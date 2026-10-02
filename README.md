## Matt Reyer

Computational biologist in Portland, Oregon. PhD in biophysics from the University of
Chicago, where my work was close to an even split between experiment and computation:
single-molecule fluorescence imaging at the bench, and kinetic modeling of the resulting
data. Most recently Computational Biologist III at the Brenden-Colson Center for
Pancreatic Care, Oregon Health & Science University (OHSU).

I work mostly on genomics pipelines — variant calling, bulk and single-cell
transcriptomics — with an emphasis on making analyses reproducible and on turning results
into something a collaborator can actually read.

### Pipelines

Each is a config-driven Snakemake workflow with a local variant sized for a laptop, a
cloud variant for larger batches, and a client-facing HTML/PDF report as a first-class
output rather than an afterthought. Each also ships a worked example: a complete run on
public data, with the figures committed, so you can see the output without installing
anything.

**[gatk-variant-calling-pipeline](https://github.com/mreyer93/gatk-variant-calling-pipeline)**
Somatic and germline short-variant calling with the Genome Analysis Toolkit (GATK),
targeting GRCh38, plus read QC and alignment. Mutect2 with orientation-bias and
contamination filtering, joint genotyping via GenomicsDBImport, copy-number and
loss-of-heterozygosity calls with FACETS, and CADD/Funcotator annotation. Where CADD's
400 GB database will not fit, the germline path can substitute AlphaMissense — currently
the ClinGen-preferred missense predictor — at roughly 640 MB.
*[Worked example](https://github.com/mreyer93/gatk-variant-calling-pipeline/blob/main/example/README.md):*
a matched tumour/normal pair, where the same tumour yields 28 passing calls against the
reference alone but only 3 once the matched normal is subtracted.

**[bulk-rnaseq-pipeline](https://github.com/mreyer93/bulk-rnaseq-pipeline)**
FASTQ to differential expression: fastp, Salmon or STAR+Salmon, tximport, DESeq2. Tool
choices follow nf-core/rnaseq so results are comparable with the community standard. The
sample sheet parser accepts the naming conventions people actually use, merges multi-lane
samples, handles mixed single- and paired-end runs, and validates the design against the
data before any compute is spent.
*[Worked example](https://github.com/mreyer93/bulk-rnaseq-pipeline/blob/main/example/README.md):*
a yeast RAP1 depletion experiment, with three conditions separating on 91% of the variance
and recognisable genes topping each contrast.

**[scrnaseq-pipeline](https://github.com/mreyer93/scrnaseq-pipeline)**
Single-cell RNA-seq following the Scanpy workflow in the single-cell best-practices book:
quality control, Scrublet doublet detection, normalization, Harmony integration, Leiden
clustering, marker genes, and provisional cell-type annotation. Starts from count matrices
or from raw reads via simpleaf/alevin-fry.
*[Worked example](https://github.com/mreyer93/scrnaseq-pipeline/blob/main/example/README.md):*
10x PBMC 3k — 4,002 cells, 9 clusters, recovering all six expected blood cell populations.

**[Regulation_Kinetics](https://github.com/mreyer93/Regulation_Kinetics)**
MATLAB and Python code for my first-author paper, Reyer et al., *Kinetic modeling reveals
additional regulation at co-transcriptional level by post-transcriptional sRNA
regulators*, Cell Reports 36(13):109764, 2021
([doi:10.1016/j.celrep.2021.109764](https://doi.org/10.1016/j.celrep.2021.109764)).
Single-molecule fluorescence in situ hybridization (FISH) and super-resolution image
analysis, plus the kinetic model fitting that produced the paper's central result: small
RNAs previously described as post-transcriptional regulators can also act
co-transcriptionally.

### On testing

Each pipeline ships a smoke test (`./test/run_test.sh`) that runs end to end on a small
public dataset in a couple of minutes, so the claim that it works is checkable rather than
asserted. The variant-calling test uses a matched tumour/normal pair from nf-core's sarek
test data, the bulk RNA-seq test uses subsampled yeast data, and the single-cell test uses
10x PBMC 3k and should recover the expected blood cell types. Each repository is explicit
about which paths are verified and which are not.

Running them is also what found the bugs worth finding: Harmony integration that silently
never applied, tumour-vs-normal calling that was disabled by default, and a `sed -i` call
that could only ever have worked on Linux.

### Background

*Genomics:* GATK best-practices variant calling, bulk and single-cell RNA-seq, Snakemake,
conda, Google Cloud.
*Modeling and analysis:* kinetic and stochastic modeling, parameter inference, DESeq2,
Scanpy, Python, R, MATLAB.
*Experimental:* single-molecule fluorescence microscopy, single-molecule FISH, STORM
super-resolution imaging, bacterial genetics.
