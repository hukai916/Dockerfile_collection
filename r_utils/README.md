# A Docker image containing common R utilities.

## Based on:
Ubuntu-16.04

## How to download:
docker pull hukai916/r_utils:0.1

## base R: 4.1.2 (version 0.1)

## BiocManager: 3.14 (version 0.1)

## Installed tools (version 0.1)
SAMtools
gawk

## Installed R packages (version 0.1)
RUN R -e 'install.packages(BiocManager")'
RUN R -e 'install.packages(devtools")'
RUN R -e 'install.packages(collections")'
RUN R -e 'install.packages(optparse")'
RUN R -e 'install.packages(stringr")'
RUN R -e 'install.packages(purrr")'
RUN R -e 'install.packages(dplyr")'
RUN R -e 'install.packages(campfin")'

RUN R -e 'BiocManager::install("GenomicFeatures")'
RUN R -e 'BiocManager::install("ShortRead")'
RUN R -e 'BiocManager::install("BSgenome")'
RUN R -e 'BiocManager::install("biomaRt")'
RUN R -e 'BiocManager::install("plyranges")'
RUN R -e 'BiocManager::install("GenomeInfoDb")'
