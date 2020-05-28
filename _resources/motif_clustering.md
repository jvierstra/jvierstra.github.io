---
title: "Non-redundant TF motif matches genome-wide"
excerpt: >
        The redundancy and overlap between various motif model databases complicate downstream analysis and interpretation. We computed the pairwise similarity for >2,000 motif models determined for both human and mouse TFs and clustered them into 286 distinct motif clusters. Within each cluster we aligned motifs to each other to generate an archetypal consensus motif.
header:
  teaser: /assets/img/teasers-02.png
date: 2020-05-15
toc: True
toc_sticky: True
---


## Overview
The redundancy and overlap between various motif model databases complicate downstream analysis and interpretation. We computed the pairwise similarity for >2,000 motif models determined for both human and mouse TFs and clustered them into 286 distinct motif clusters. Within each cluster we aligned motifs to each other to generate an archetypal consensus motif.


{% include figure image_path='/assets/img/clustering_workflow.png' alt='Motif clustering workflow' %}

We scanned the entire genome using each motif model (all 2,179 models), and then repositioned the coordinates of each genomic match according to each motifs relative position to its archetypal model. After adjusting the coordinates of each motif match, we remove all duplicates (same position and motif cluster), retaining a single match at any position in the genome.

{% include figure image_path='/assets/img/motif_browser_shot.png' alt='Browsershot of motif UCSC track' %}

Some key **advantages** of the this approach:

- Allow sharing of information about TFBS from many models. Any match to an individual model is a match to the cluster
- No information is lost about individual motif model matches. The individual motif matches are fully represented in each cluster archetype match
- Agnostically identifies errors and mislabeling within motif databases

Some notable **caveats/shortcomings** that should be considered:

- No optimization on the bit-score/*p*-value threshold for each motif model
- Cluster tree is cut at an abitrary height (chosen by the *look and feel* of the clusters)

## Included motif databases (v1.0)

JASPAR 2018
: Models derived from published and experimentally defined transcription factor binding sites for eukaryotes. The "CORE vertabrates" non-redundant PFMs were included.
- Motif models [[MEME-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/jaspar2018/JASPAR2018_CORE_vertebrates_non-redundant_pfms.meme)]
- Motif weblogos [[EPS-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/jaspar2018/logos)]


Taipale 2013 HT-SELEX
: Models derived from high-throughput SELEX experiments using bacterially expressed human and mouse DNA-binding domains. See Table S3 from publication.
- Motif models [[MEME-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/jolma2013/jolma2013.meme)]
- Motif weblogos [[EPS-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/jolma2013/logos)]

HOCOMOCO version 11
: Models derived from ChIP-seq data (680 human + 452 mouse TFs)
- Motif models [Human: [MEME-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/hocomoco_v11/HOCOMOCOv11_core_HUMAN_mono_meme_format.meme), Mouse: [MEME-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/hocomoco_v11/HOCOMOCOv11_core_MOUSE_mono_meme_format.meme)]
- Motif weblogos: [[EPS-format](https://resources.altius.org/~jvierstra/projects/motif-clustering/databases/hocomoco_v11/logos)]

## Notes

Motifs scans were performed with [MOODS]()

```bash
python2 moods_dna.py  \
	--sep ";" -s h38.fa --p-value 0.0001 \
	--lo-bg 2.977e-01 2.023e-01 2.023e-01 2.977e-01 \
	-m PFM_FILE -o OUTFILE
```

## Downloads

### v1.0 (2020-4-14)

Clustering of 2179 motif models (3 databases above)

- Motif clustering annotations ([excel file](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0/motif_annotations.xlsx))
- Genome-wide motif scans

	| Genome build | Full scans | Archetype scans |
	|-------|--------|---------|
	| Human (hg38)| [bed.gz](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0) + [tabix](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0), [bigBed](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0) | [bed.gz](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0) + [tabix](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0), [bigBed](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0)  |
	| Mouse (mm10)| [bed.gz](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0) + [tabix](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0), [bigBed](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0)  | [bed.gz](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0) + [tabix](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0), [bigBed](https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0)  |

	{% include warning.html content="These files are massive (>40Gb)!" %}

### File format descriptions


**Full scans**

|| Column | Example | Description |
|---|-------|--------|---------|
|1|`contig`|chr1 | Chromosome|
|2|`start`|10003 | Start position (0-based) |
|3|`stop`|10023 | End position |
|4|`motif`| RREB1_MA0073.1 | Motif model name|
|5|`match_score`| 3.75359698976 | MOODS match score |
|6|`strand`| + | Matching strand |
|7|`seq`|CCCTAACCCTAACCCTAACC | Genomic sequence of match|

**Archetype scans**

|| Column | Example | Description |
|---|-------|--------|---------|
|1|`contig`|chr1 | Chromosome|
|2|`start`|10005 | Start position (0-based) |
|3|`stop`|10022 | End position |
|4|`motif_cluster`| KLF/SP/2 | Motif cluster name|
|5|`match_score`| 3.7536 | MOODS match score for best cluster match |
|6|`strand`|+ | Matching strand |
|7|`best_model`|RREB1_MA0073.1 | Best matching motif model from cluster|
|8|`num_models`|1 | Number of motif model from cluster with a match|


## Browser tracks

To visualize the motif matches in the genome, I have created a [trackhub](https://genome.ucsc.edu/goldenPath/help/hubQuickStart.html) for the UCSC Genome Browser. Briefly, you can load this track by navigating on any UCSC browser instance to ```Data > Trackhubs``` and then copying and pasting the following URL:

```
https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0/hub.txt
```

Alternatively, you can [click here](http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38&hubUrl=https://resources.altius.org/~jvierstra/projects/motif-clustering/releases/v1.0/hub.txt) to automatically load this trackhub at the Genome Browser hosted at UCSC.

## Code

Software and scripts are available at [GitHub](http://www.github.com/jvierstra/motif-clustering) to reproduce the motif clustering.

## Citation

If you use this resource in your research, please kindly cite:

Vierstra J *et al.* (2020). [Global reference mapping and dynamics of human transcription factor footprints.](https://www.biorxiv.org/content/10.1101/2020.01.31.927798v1) ***bioRxiv***.

## Additional resources

Admittedly, I am far from the first person to cluster motif models. In any case, this resource is/was largely intended to aid in visualizing and "surfing" the genome browser and to accompany other annotations (e.g., ChIP-seq, DGF, etc).

I have compiled a list of publications, websites and databases pertaining the the curation of motif models which may of be of use. If you find something useful, please contact me and I will include it here.

**Clustering:**
- [RSAT](http://jaspar.genereg.net/matrix-clusters/): Clustering methodology used as part of JASPAR project ([publication link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5737723/))
- [TOMTOM](http://meme-suite.org/): Software tools used here for clustering ([publication link](https://pubmed.ncbi.nlm.nih.gov/17324271/)) 

**Motif curation:**
- [JASPAR](http://jaspar.genereg.net/)
- [HOCOMOCO](https://hocomoco11.autosome.ru/)
- [CisBP](http://cisbp.ccbr.utoronto.ca/) (Tim Hughes' group)
- [Lambert *et al.*](https://pubmed.ncbi.nlm.nih.gov/29425488/): "The human transcription factors" (Supplementary Table 1)
- [UniProbe](http://thebrain.bwh.harvard.edu/uniprobe/) (Martha Bulyk)


