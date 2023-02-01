FROM continuumio/miniconda3:22.11.1

# install R with miniconda:
RUN conda install -c conda-forge r-base=4.2.2

# install build-essential package: a lot of R packages depend on build-essential
RUN apt-get update -q && apt-get install -y build-essential

# install tools required for msigdbr:
RUN R -e 'install.packages("BiocManager", repos = "http://cran.us.r-project.org")'
RUN R -e 'install.packages("zip", repos = "http://cran.us.r-project.org")'
RUN R -e 'BiocManager::install("dplyr")'
RUN R -e 'BiocManager::install("msigdbr")'
