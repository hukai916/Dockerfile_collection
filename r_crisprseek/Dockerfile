## Emacs, make this -*- mode: sh; -*-

## Build a R environment that uses Ubuntu 16.04 (Xenial) otherwise kernel too new to run on HPCC
## Steps:
#### 1. install latest released R (4.1.0)
#### 2. install other essential tools: git
#### 3. install required R packages:
#### BiocManager, CRISPRseek, TxDb.Hsapiens.UCSC.hg38.knownGene, BSgenome.Hsapiens.UCSC.hg38

## Step1: below is to install the latest base R: adapted from https://github.com/rocker-org/rocker/blob/master/r-ubuntu/focal/Dockerfile
#### Adapted:
#### Note that used 4.1.0-1.1604.0 instead of 4.0.5-1.1604.0
#### Note that used BiocManager v3.13 for R 4.1.0
#### FROM ubutu:16.04;
####     apt-transport-https \
####     libjpeg-turbo8-dev \
####     libpng-dev \
####     libbz2-dev \
####     liblzma-dev \
## Above 4 are now required for ShortRead
####     build-essential \
## Above 1 is now required to have make utility
####     r-recommended=4.0.5-1.1604.0 \
####     r-base-core=4.0.5-1.1604.0 \
## Above two are necessary :
## r-recommended=4.0.5-1.1604.0 is required, otherwise installed r-base 4.0.5 will be auto-upgraded to latest version
## r-base-core=4.0.5-1.1604.0 is required, otherwise will attempt to install recommended r-base version and fail

FROM ubuntu:16.04

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/rocker-org/r-ubuntu" \
      org.label-schema.vendor="Rocker Project" \
      maintainer="Dirk Eddelbuettel <edd@debian.org>"

## Set a default user. Available via runtime flag `--user docker`
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly).
RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker \
	&& addgroup docker staff

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		software-properties-common \
    dirmngr \
    ed \
		less \
		locales \
		vim-tiny \
		wget \
		ca-certificates \
    apt-transport-https \
    build-essential \
    libjpeg-turbo8-dev \
    libpng-dev \
    libbz2-dev \
    liblzma-dev \
  && add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0" \
  && add-apt-repository --enable-source --yes "ppa:c2d4u.team/c2d4u4.0+"

## Configure default locale, see https://github.com/rocker-org/rocker/issues/19
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

## This was not needed before but we need it now
ENV DEBIAN_FRONTEND noninteractive

## Otherwise timedatectl will get called which leads to 'no systemd' inside Docker
ENV TZ UTC

# Now install R and littler, and create a link for littler in /usr/local/bin
# Default CRAN repo is now set by R itself, and littler knows about it too
# r-cran-docopt is not currently in c2d4u so we install from source
## Below are added by Kai in order to install the specific R 4.0.5:
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  littler \
  r-base=4.1.0-1.1604.0 \
  r-recommended=4.1.0-1.1604.0 \
  r-base-core=4.1.0-1.1604.0 \
  r-base-dev=4.1.0-1.1604.0 \
  && ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*
## End of Step1

## Step2: install other essential tools: git
#### added libxml2-dev for installing R XML package
#### added libssl-dev for installing rtracklayer
#### added pandoc for installing markdown (suggested by ChIPpeakAnno but used to build the vignette)
#### comment out svn co but use hard copy since the svn always get the latest R-devel which the VERSION may change unpredictably.

RUN apt-get update && apt-get install -y --no-install-recommends git
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    libxml2-dev \
    libssl-dev \
    pandoc \
    libcurl4-openssl-dev # required for installation of curl -> campfin; also required for ShortRead

## Step3: install required R packages:
## Step3: install BiocManager-devel: 3.13
# RUN apt-get update && apt-get install gfortran # requried by ATACseqQC installation. No, if r-base-dev is installed
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'BiocManager::install(version = "3.13")' # For unknown reason, while Mac is 3.12, linux will be 3.11, and there S4Vector is 0.26.1, but we need 0.28.1 for ShortRead, so upgrade BiocManager first
RUN R -e 'BiocManager::install("CRISPRseek")'

RUN R -e 'BiocManager::install("TxDb.Hsapiens.UCSC.hg38.knownGene")'
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg38")'
RUN R -e 'BiocManager::install("org.Hs.eg.db")'

RUN R -e 'BiocManager::install("TxDb.Mmusculus.UCSC.mm10.knownGene")'
RUN R -e 'BiocManager::install("org.Mm.eg.db")'
RUN R -e 'BiocManager::install("BSgenome.Mmusculus.UCSC.mm10")'
