# A docker image containing R-4.1.0 (See Notes1) built on Xenial (Ubuntu-16.04)
**Docker image:** docker pull hukai916/r_crisprseek:0.1

This image contains R packages for CRISPRseek.

**R packages**:
- BiocManager
- CRISPRseek
- TxDb.Hsapiens.UCSC.hg38.knownGene
- BSgenome.Hsapiens.UCSC.hg38
- etc.

Build step summary:
1. install latest released R (4.1.0)
2. install other essential tools: git
3. install required R packages:
BiocManager, CRISPRseek, TxDb.Hsapiens.UCSC.hg38.knownGene, BSgenome.Hsapiens.UCSC.hg38, etc.
