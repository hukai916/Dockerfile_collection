# A docker image containing R-4.1.0 built on Xenial (Ubuntu-16.04)
**Docker image:** docker pull hukai916/r_sc:0.1

This image contains R packages: future, Seurat, Signac.

**R packages**:
- BiocManager
- future
- Seurat
- Signac

Build step summary:
1. install latest released R (4.1.0)
2. install other essential tools: git
3. install required R packages:
BiocManager, future, Seurat, Signac
