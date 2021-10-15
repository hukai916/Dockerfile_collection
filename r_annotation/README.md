# A docker image containing R-4.0.5 (See Notes1) built on Xenial (Ubuntu-16.04)
**Docker image:** docker pull hukai916/r_annotation:0.1

This image contains both R packages and conda tools:

**R packages**:
- ATACseqQC
- ChIPpeakAnno
- etc.

**Conda tools**:
- bedtools

Build step summary:
1. install latest released R (4.0.5)
2. install other essential tools: git
3. install required R packages:
BiocManager, ATACseqQC, ChIPpeakAnno, etc.
4. install miniconda3
5. install conda tools:
bedtools
