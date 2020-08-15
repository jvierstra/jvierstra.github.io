---
title: "Digital genomic footprinting"
subtitle: Data from Vierstra et al. 2020 Nature
excerpt: >
        Genomic DNase I footprinting enables quantitative, nucleotide-resolution delineation of sites of transcription factor occupancy within native chromatin. We combined sampling of >67 billion uniquely mapping DNase I cleavages from >240 human cell types and states to index human genomic footprints.
header:
  teaser: /assets/img/teasers-01.png
date: 2020-07-10
toc: True
toc_sticky: True
---

Genomic DNase I footprinting enables quantitative, nucleotide-resolution delineation of sites of transcription factor occupancy within native chromatin. We combined sampling of >67 billion uniquely mapping DNase I cleavages from >240 human cell types and states to index, with unprecedented accuracy and resolution, human genomic footprints and thereby the sequence elements that encode transcription factor recognition sites. 


{% include figure image_path='/assets/img/dgf_example.png' alt='Digital genomic footprinting' %}

## Downloads

A web directory containing all of the material for download is available [here](https://resources.altius.org/~jvierstra/projects/footprinting.2020/). Note: the same versions of some of the processed data is also persistently hosted at [ZENODO](https://doi.org/10.5281/zenodo.3603548) and the [ENCODE Project Portal](http://www.encodeproject.org). All files correspond to human genome build `GRChr38/hg38`. See below for [file format descriptions](#appendix-file-format-descriptions).

- **Metadata containing information about the 243 biosamples analyzed** ([excel](https://resources.altius.org/~jvierstra/projects/footprinting.2020/Supplementary_Table_1.xlsx)) (corresponds to manuscript *Supplementary Table 1*)

- **Footprints identified in individual datasets** ([directory listing](https://resources.altius.org/~jvierstra/projects/footprinting.2020/per.dataset/))  
  For each of the 243 datasets you will find the files outlined below. We have organized the files into sub-directories corresponding to each dataset.

  | Filename  | Description |
  |-----------|-------------|
  | `reads.(bam|bam.bai)` | BAM alignment file from DNase I experiment |
  | `hotspots.bed.starch` | DNase I hotspots |
  | `peaks.bed.starch` | DNase I peaks |
  | `dm.json` | Dispersion model file |
  | `qc.pdf` | QC plots of dispersion model |
  | `interval.all.bedgraph.(gz|gz.tbi|starch)` | Per-nucleotide footprint statistics |
  | `interval.all.obs.bw` | Observed cleavage counts (bigWig) |
  | `interval.all.exp.bw` | Expected cleavage counts (bigWig) |
  | `interval.all.lnpval.bw` | Per-nucleotide *p*-value (bigWig) |
  | `interval.all.winlnpval.bw` | Windowed *p*-value (bigWig) |
  | `interval.all.fpr.bw` | FPR adjusted *p*-values (bigWig) |
  | `interval.all.fps.*.(bed|bed.gz|bed.gz.tbi|bb)` | FPR thresholded footprints |

  Note: starch format requries [BEDOPS](http://bedops.readthedocs.io) to decompress

- **Per-nucleotide posterior footprint probability** ([gzipped bedgraph](https://resources.altius.org/~jvierstra/projects/footprinting.2020/posteriors/posteriors.per-nt.bed.gz) and [tabix index](https://resources.altius.org/~jvierstra/projects/footprinting.2020/posteriors/posteriors.per-nt.bed.gz.tbi))  
  Each row contains the –log transformed posterior probability of footprint in each of the 243 datasets (used to create **Fig. 1** of associated manuscript). Columns (biosamples) are in the same order as sample metadata file above (Supplementary Table 1). *Genomic positions were excluded if **no** individual sample has a posterior footprint probability > 0.8.*  

  Note that this file is massive (\~66Gb) and we have provided a TABIX-index alongside to facilitate remote access using ```tabix```:
```
[jvierstra@test0 $] tabix https://resources.altius.org/~jvierstra/projects/footprinting.2020/posteriors/posteriors.per-nt.bed.gz chr19:45,001,882-45,002,279
chr19   45001881        45001882        0.0     1.585874187526315e-08   7.332801033044234e-12   1.1864528914884431e-08  2.4502702181905534e-05 ...
chr19   45001886        45001887        7.63844553830495e-07    1.0648232517951328e-08  0.0     2.842170943040401e-14   6.063725059846092e-07 ...
chr19   45001887        45001888        0.026677374186842684    2.448578051428285e-07   5.897504706808832e-13   4.969464839632565e-10   2.277549524620781e-05 ...
...
```

  Additionally, an example of how to remotely access and plot these data using Python can be found [here](https://footprint-tools.readthedocs.io/en/latest/examples/plot.html).

- **Consensus footprints** ([gzipped bed](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/consensus_footprints_and_motifs_hg38.bed.gz) and [tabix index](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/consensus_footprints_and_motifs_hg38.bed.gz.tbi))  
  Re-analysis of the individual datasets using an Emperical Bayesian framework.

  - **Footprinted motif archetypes** ([gzipped bed](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/collapsed_motifs_overlapping_consensus_footprints_hg38.bed.gz) and [tabix index](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/collapsed_motifs_overlapping_consensus_footprints_hg38.bed.gz.tbi))  
    Motif archetypes matches that overlap consensus footprints (see [motif clustering](/resources/motif_clustering) for more information) 

  - **Footprint-by-biosample matrix**  
    Matrix delineating the occupancy of individual consensus footprints (row) in individual biosamples (columns). Biosamples (columns) in same order as sample metadata table.

      - Binary occupied/unoccupied matrix ([gzipped tsv](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/consensus_index_matrix_binary_hg38.txt.gz))
      - Full posterior probabilities ([gzipped tsv](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/consensus_index_matrix_full_hg38.txt.gz))

- **Single nucleotide variants tested for allelic imbalance**  
  *De novo* genotypes derived from 147 individuals and allelic read counts for each variant in all 243 datasets. The assignment of each biosample to an individual is found in Supplementary Table 1 (above).

    - Genotype and allelic read depth for each biosample ([gzipped vcf](https://resources.altius.org/~jvierstra/projects/footprinting.2020/allelic_imbalance/genotypes.vcf.gz) and [tabix index](https://resources.altius.org/~jvierstra/projects/footprinting.2020/allelic_imbalance/genotypes.vcf.gz.tbi))  
    The file format is self-explantory (see header for description via `bcftools view -h genotypes.vcf.gz`)

    - Variants tested for imbalance (combining data by genotype) using Beta-binomial distribution ([gzipped bed](https://resources.altius.org/~jvierstra/projects/footprinting.2020/allelic_imbalance/tested_snvs_padj.bed.gz) and [tabix index](https://resources.altius.org/~jvierstra/projects/footprinting.2020/allelic_imbalance/tested_snvs_padj.bed.gz.tbi))

## Visualization

All of the DGF data listed above can be loaded/visualized in the UCSC Genome Browser with the following trackhub (copy & paste into "My Hubs"):

```
https://resources.altius.org/~jvierstra/projects/footprinting.2020/hub.txt
```

Alternatively, you can [click here](http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38&hubUrl=https://resources.altius.org/~jvierstra/projects/footprinting.2020/hub.txt){:target="_blank"} to automatically load this trackhub at the Genome Browser hosted at UCSC.


## Code & Tutorials

Software and scripts are available at [GitHub](http://github.com/jvierstra/footprint-tools). 

Documentation is hosted at [Read the docs](http://footprint-tools.readthedocs.io). Included in the documentation are [examples](https://footprint-tools.readthedocs.io/en/latest/examples.html) of how to remotely access, manipulate and visualize genomic footprinting data.

## Citation

If you use this resource in your research, please cite:

[Vierstra2020](https://doi.org/10.1038/s41586-020-2528-x) Vierstra, J., Lazar, J., Sandstrom, R. *et al.* Global reference mapping of human transcription factor footprints. *Nature* **583**, 729–736 (2020)

## Acknowledgements

This work was funded through the NHGRI ENCODE Project (NIH grants U54HG007010 and 5UM1HG009444)

## Appendix: File format descriptions

**Hotspots and peaks**

See [hotspot2 documentation](https://github.com/Altius/hotspot2) for file descriptions.

**Per-nucleotide footprint statistics for individual datasets** (`interval.all.bedgraph.gz`)
  
|| Column | Example | Description |
|---|-------|--------|---------|
|1|`contig`|chr1 | Chromosome|
|2|`start`|9823494 | Start position (0-based) |
|3|`stop`|9823495 | End position (`start+1`) |
|4|`obs`| 24 | Observed DNase I cleavages (both strands combined)|
|5|`exp`| 56 | Expected DNase I cleavages |
|6|`lnp`|2.1 | –log *p*-value lower-tail negative binomial|
|7|`winlp`|14.5 | –log *p*-value windowed test (Stouffer's *Z*)|
|8|`fdr`| 0.0014 | Empircal false-discovery rate |

**Consensus footprints** (`consensus_footprints_and_motifs_hg38.bed.gz`)

|| Column | Example | Description |
|---|-------|--------|---------|
|1|`contig`|chr10 | Chromosome|
|2|`start`|97320044 | Start position (0-based) |
|3|`stop`|97320056 | End position (`start+1`) |
|4|`identifier`|10.754379.4 | Unique identifier (DHS_chom#.DHS_position%.fp_position%; DHS_chrom#.DHS_position% = DHS index identifier) |
|5|`mean_signal`|55.865317 | Mean footprint -log(1-posterior) across biosamples ("confidence score") |
|6|`num_samples`|9 |  Number of unique biosamples contributing to this index footprint (posterior >= 0.99) |
|7|`num_fps`|9 |Number of unique footprints across all samples that contributed to this consensus footprint |
|8|`width`| 12 | Width of consensus footprint (column 3-column 2)|
|9|`summit_pos`| 97320049 | Estimated footprint summit position|
|10|`core_start`| 97320042 | Start position of core-region containing 95% of per-biosample summits|
|11|`core_end`| 97320053 | End position of core-region containing 95% of per-biosample summits|
|12|`motif_clusters`| CTCF;KLF/SP/2;ZNF563 | [Non-redundant motif archetype matches](/resources/motif_clustering) w/ 90% overlap, `;` delimited|

**Footprinted motif archetypes** (`collapsed_motifs_overlapping_consensus_footprints_hg38.bed.gz`)

|| Column | Example | Description |
|---|-------|--------|---------|
|1|  `contig`       |chr1        |Chromosome|
|2|  `start`         |1782520     |Start position (0-based)|
|3|  `end`           |1782770     |End position|
|4|  `motif_cluster` |TBX/4       |Motif cluster name|
|5|  `score`         |0           |BED-score field (not used)|
|6|  `strand`        |+           |Strand (+ or -)|
|7|  `thickStart`    |1782520     |Same as 'start'|
|8|  `thickEnd`      |1782770     |Same as 'end'|
|9|  `itemRgb`       |0,28,255    |RGB string for UCSC browser|
|10| `best_model`    |TBX20_TBX_1 |Best matching motif model from cluster|
|11| `match_score`   |5.4513      |MOODS match score for best cluster match|
|12| `DBD`           |TBX         |DNA binding domain family|
|13| `num_models`    |2           |Number of motif models from cluster with a match|


For more information about the motif archetypes see [here](/resources/motif_clustering).

**Variants tested for imbalance** (`tested_snvs_padj.bed.gz`)

|| Column | Example | Description |
|---|-------|--------|---------|
|1|  `contig`        |chr10        |Chromosome|
|2|  `start`         |404425     |Start position (0-based)|
|3|  `end`           |404426     |End position|
|4|  `ref`           |G           |Reference allele|
|5|  `alt`           |T           |Alternative allele|
|6|  `total_reads`   |558         |Number of total reads over variant in het. samples|
|7|  `ref_reads`     |196         |Number of reads mapped to reference allele|
|8|  `num_hets`      |6           |Number of heterozygous biosamples|
|9|  `allelic_ratio` |0.3513      |Proportion reads mapping to reference allele|
|10| `bb_test_stat`    |0.0017 |Beta-binomial test statistic|
|11| `adj_p`   |0.0468120827793      |Adjusted *p*-value|
|12| `dhs`           |1         |Overlapping DHS peak (binary indicator)|
|13| `fps`    |1           |Overlapping consensus footprint (binary indicator)|


