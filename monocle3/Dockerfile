FROM r-base:4.4.1

CMD ["bash"]

# install some essential libraries for installing devtools
RUN R -e 'install.packages("BiocManager")' 
RUN apt-get update && apt-get install -y \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev \
    build-essential \
    libfontconfig1-dev \
    libcairo2-dev \
    libpango1.0-dev \
    libglib2.0-dev \
    libgit2-dev \
    libfreetype6-dev \
    libpng-dev \
    libtiff5-dev \
    libjpeg-dev
RUN R -e 'install.packages("devtools")'

# install monocle3: https://cole-trapnell-lab.github.io/monocle3/docs/installation/
RUN R -e 'install.packages("BiocManager")'
RUN apt-get update && apt-get install -y cmake
RUN R -e 'install.packages("lme4")'
RUN R -e 'BiocManager::install(c("HDF5Array", "BiocGenerics", "DelayedArray", "DelayedMatrixStats", "limma", "S4Vectors", "SingleCellExperiment", "SummarizedExperiment", "batchelor", "terra", "ggrastr"))'
RUN apt-get update && apt-get install -y libgdal-dev 
RUN R -e 'BiocManager::install("terra")'
RUN apt-get update && apt-get install libudunits2-dev -y
RUN R -e 'devtools::install_github("cole-trapnell-lab/monocle3")'

# install other related packages
RUN R -e 'remotes::install_github("satijalab/seurat", "seurat5")'
RUN R -e 'install.packages("R.utils")'
RUN R -e 'remotes::install_github("satijalab/seurat-wrappers", "seurat5")'