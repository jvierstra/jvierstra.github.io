---
title: "Digital genomic footprinting"
subtitle: Data from Vierstra et al. 2020 bioRxiv
excerpt: >
        Genomic DNase I footprinting enables quantitative, nucleotide-resolution delineation of sites of transcription factor occupancy within native chromatin. We combined sampling of >67 billion uniquely mapping DNase I cleavages from >240 human cell types and states to index human genomic footprints.
header:
  teaser: /assets/img/teasers-01.png
date: 2020-05-15
toc: True
toc_sticky: True
---

Genomic DNase I footprinting enables quantitative, nucleotide-resolution delineation of sites of transcription factor occupancy within native chromatin. We combined sampling of >67 billion uniquely mapping DNase I cleavages from >240 human cell types and states to index, with unprecedented accuracy and resolution, human genomic footprints and thereby the sequence elements that encode transcription factor recognition sites. 


{% include figure image_path='/assets/img/dgf_example.png' alt='Digital genomic footprinting' %}

## Downloads

A web directory containing all of the material for download is available [here](https://resources.altius.org/~jvierstra/projects/footprinting.2020/). Note: the same versions of some of the processed data is also persistently hosted at [ZENODO](https://doi.org/10.5281/zenodo.3603548). All files correspond to human genome build `GRChr38/hg38`. See below for [file format descriptions](#appendix-file-format-descriptions).

- **Metadata containing information about the 243 biosamples analyzed** ([excel](), [tsv]()) (corresponds to manuscript *Extended Data Table 1*)

- **Footprints identified in individual datasets** ([directory listing](https://resources.altius.org/~jvierstra/projects/footprinting.2020/per.dataset/))  
  For each of the 243 datasets you will find the files outlined below. We have organized the files into sub-directories corresponding to each dataset.

  | Filename  | Description |
  |-----------|-------------|
  | `reads.bam` | BAM alignment file from DNase I experiment |
  | `hotspots.bed.starch` | DNase I hotspots |
  | `peaks.bed.starch` | DNase I peaks |
  | `dm.json` | Dispersion model file |
  | `qc.pdf` | QC plots of dispersion model |
  | `interval.all.bedgraph.(gz|starch)` | Per-nucleotide footprint statistics |
  | `interval.all.obs.bw` | Observed cleavage counts (bigWig) |
  | `interval.all.exp.bw` | Expected cleavage counts (bigWig) |
  | `interval.all.lnpval.bw` | Per-nucleotide *p*-value (bigWig) |
  | `interval.all.winlnpval.bw` | Windowed *p*-value (bigWig) |
  | `interval.all.fpr.bw` | FPR adjusted *p*-values (bigWig) |
  | `interval.all.fps.*.(bed|bed.gz|bb)` | FPR thresholded footprints |

  Note: starch format requries [BEDOPS](http://bedops.readthedocs.io) to decompress

- **Consensus footprints** ([gzipped bed](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/Consensus_footprints_and_motifs_hg38.bed.gz))  
  Re-analysis of the individual datasets using an Emperical Bayesian framework.

  - **Footprinted motif archetypes** ([gzipped bed](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/Collapsed_motifs_overlapping_consensus_footprints_hg38.bed.gz))  
    Motif archetypes matches that overlap consensus footprints (see [motif clustering](/resources/motif_clustering) for more information) 

  - **Footprint-by-biosample matrix**  
    Matrix delineating the occupancy of individual footprints (row) in individual biosamples (columns). Biosamples (columns) in same order as sample metadata table.

      - Binary occupied/unoccupied matrix ([gzipped tsv](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/Consensus_index_matrix_binary_hg38.bed.gz))
      - Posterior matrix (full probabilities) ([gzipped tsv](https://resources.altius.org/~jvierstra/projects/footprinting.2020/consensus.index/Consensus_index_matrix_full_hg38.bed.gz))

## Visualization

All of the DGF data listed above can be loaded/visualized in the UCSC Genome Browser with the following trackhub (copy & paste into "My Hubs"):

```
https://resources.altius.org/~jvierstra/projects/footprinting.2020/hub.txt
```

Alternatively, you can [click here](http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38&hubUrl=https://resources.altius.org/~jvierstra/projects/footprinting.2020/hub.txt){:target="_blank"} to automatically load this trackhub at the Genome Browser hosted at UCSC.


## Code

Software and scripts are available at [GitHub](http://github.com/jvierstra/footprint-tools). 

Documentation is hosted at [Read the docs](http://footprint-tools.readthedocs.io).

## Citation

If you use this resource in your research, please kindly cite:

Vierstra J *et al.* (2020). [Global reference mapping and dynamics of human transcription factor footprints.](https://www.biorxiv.org/content/10.1101/2020.01.31.927798v1) *bioRxiv*.

## Appendix: File format descriptions

**Hotspots and peaks**

See [hotspot2 documentation](https://github.com/Altius/hotspot2) for file descriptions.

**Per-nucleotide footprint statistics** (`interval.all.fps.bedgraph`)
  
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

**Consensus footprints** (`Consensus_footprints_and_motifs_hg38.bed.gz`)

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
|11|`core_end`| 97320053 | [End position of core-region containing 95% of per-biosample summits|
|12|`motif_clusters`| CTCF;KLF/SP/2;ZNF563 | [Non-redundant motif archetype matches](/resources/motifs) w/ 90% overlap, `;` delimited|

**Footprinted motif archetypes** (`Collapsed_motifs_overlapping_consensus_footprints_hg38.bed.gz`)

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
|9| `itemRgb`      |0,28,255     |RGB string for UCSC browser|
|10| `best_model`    |TBX20_TBX_1 |Best matching motif model from cluster|
|11| `match_score`   |5.4513      |MOODS match score for best cluster match|
|12| `DBD`           |TBX         |DNA binding domain family|
|13| `num_models`    |2           |Number of motif models from cluster with a match|


For more information about the motif archetypes see [here](/resources/motifs).



