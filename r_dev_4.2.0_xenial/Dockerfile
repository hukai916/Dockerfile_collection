## Emacs, make this -*- mode: sh; -*-

## Build a Rdevel test environment that uses Ubuntu 16.04 (Xenial) otherwise kernel too new to run on HPCC
## Steps:
#### 1. install latest released R (4.0.5)
#### 2. install Rdevel (4.1.0)
#### 3. install BiocManager-devel (3.13)
#### 4. install other essential tools: git

## Step1: below is to install the latest base R: adapted from https://github.com/rocker-org/rocker/blob/master/r-base/latest/Dockerfile
#### Adapted:
#### FROM ubutu:16.04;

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
RUN apt-get update \
        && apt-get install -y --no-install-recommends \
                 littler \
 		 r-base \
 		 r-base-dev \
 		 r-recommended \
  	&& ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*=
## End of Step1

## Step2: install Rdevel: adapted from https://github.com/rocker-org/rocker/blob/master/r-devel/Dockerfile
#### Adapted:
#### removed -t unstable from apt-get
#### removed unstale from libcairo2-dev/unstable
#### comment out svn co but use hard copy since the svn always get the latest R-devel which the VERSION may change unpredictably.

## Needed in case a base package has an interactive question
## (as e.g. base-passwd in Oct 2020)
ENV DEBIAN_FRONTEND noninteractive

## Remain current
RUN apt-get update -qq \
	&& apt-get dist-upgrade -y

## From the Build-Depends of the Debian R package, plus subversion
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		bash-completion \
		bison \
		debhelper \
		default-jdk \
		g++ \
		gcc \
		gdb \
		gfortran \
		groff-base \
		libblas-dev \
		libbz2-dev \
		libcairo2-dev \
		libcurl4-openssl-dev \
		libjpeg-dev \
		liblapack-dev \
		liblzma-dev \
		libncurses5-dev \
		libpango1.0-dev \
		libpcre3-dev \
		libpng-dev \
		libreadline-dev \
		libtiff5-dev \
		libx11-dev \
		libxt-dev \
		mpack \
		subversion \
		tcl8.6-dev \
		texinfo \
		texlive-base \
		texlive-extra-utils \
		texlive-fonts-extra \
		texlive-fonts-recommended \
		texlive-generic-recommended \
		texlive-latex-base \
		texlive-latex-extra \
		texlive-latex-recommended \
		tk8.6-dev \
		x11proto-core-dev \
		xauth \
		xdg-utils \
		xfonts-base \
		xvfb \
		zlib1g-dev

## Check out R-devel
#RUN cd /tmp \
#&& svn co https://svn.r-project.org/R/trunk R-devel

## We need R 4.1.0 since BiocManager-devel requires that specific version, however svn always retrieves the latest R-devel, as of 04-20-2021, it is R4.2.0, therefore use hard copy fro R daily (https://stat.ethz.ch/R/daily/).
COPY R-devel_2022-01-07.tar.gz /tmp
RUN cd /tmp \
&& tar -xf R-devel_2022-01-07.tar.gz \
&& rm R-devel_2022-01-07.tar.gz

## Build and install according the standard 'recipe' I emailed/posted years ago
RUN cd /tmp/R-devel \
	&& R_PAPERSIZE=letter \
		R_BATCHSAVE="--no-save --no-restore" \
		R_BROWSER=xdg-open \
		PAGER=/usr/bin/pager \
		PERL=/usr/bin/perl \
		R_UNZIPCMD=/usr/bin/unzip \
		R_ZIPCMD=/usr/bin/zip \
		R_PRINTCMD=/usr/bin/lpr \
		LIBnn=lib \
		AWK=/usr/bin/awk \
		CFLAGS=$(R CMD config CFLAGS) \
		CXXFLAGS=$(R CMD config CXXFLAGS) \
	./configure --enable-R-shlib \
               --without-blas \
               --without-lapack \
               --with-readline \
               --without-recommended-packages \
               --program-suffix=dev \
	&& make \
	&& make install \
	&& rm -rf /tmp/R-devel

## Set Renviron to get libs from base R install
RUN echo "R_LIBS=\${R_LIBS-'/usr/local/lib/R/site-library:/usr/local/lib/R/library:/usr/lib/R/library'}" >> /usr/local/lib/R/etc/Renviron

## Set default CRAN repo
RUN echo 'options(repos = c(CRAN = "https://cran.rstudio.com/"), download.file.method = "libcurl")' >> /usr/local/lib/R/etc/Rprofile.site

## Copy 'checkbashisms' (as a local copy from devscripts package)
COPY checkbashisms /usr/local/bin

RUN cd /usr/local/bin \
# && mv R Rdevel \
# && mv Rscript Rscriptdevel \
&& ln -s Rdevel RD \
&& ln -s Rscriptdevel RDscript
## End of Step2

## Step3: install BiocManager-devel: 3.13
RUN R -e 'install.packages("BiocManager")' \
&& R -e 'BiocManager::install(version = "devel")' \
&& R -e 'BiocManager::valid()'
## End of Step3

## Step4: install other essential tools: git
#### added libxml2-dev for installing R XML package
#### added libssl-dev for installing rtracklayer
#### added pandoc for installing markdown (suggested by ChIPpeakAnno but used to build the vignette)
RUN apt-get update && apt-get install -y --no-install-recommends git
RUN apt-get update \
    	&& apt-get install -y --no-install-recommends \
    libxml2-dev \
    libssl-dev \
    pandoc


# Below are required for testing the installation of CRISPRseek 1.35.2 with bioc2.15:
RUN R -e 'install.packages("dplyr")'
RUN R -e 'install.packages("seqinr")'
RUN R -e 'install.packages("data.table")'
RUN R -e 'install.packages("hash")'
RUN R -e 'install.packages("reticulate")'

RUN R -e 'BiocManager::install("BiocGenerics")'
RUN R -e 'BiocManager::install("Biostrings")'
RUN R -e 'BiocManager::install("S4Vectors")'
RUN R -e 'BiocManager::install("IRanges")'
RUN R -e 'BiocManager::install("BSgenome")'
RUN R -e 'BiocManager::install("rhdf5")'
RUN R -e 'BiocManager::install("XVector")'
RUN R -e 'BiocManager::install("keras")'
RUN R -e 'BiocManager::install("mltools")'
RUN R -e 'BiocManager::install("RUnit")'
RUN R -e 'BiocManager::install("BiocStyle")'
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg19")'
RUN R -e 'BiocManager::install("TxDb.Hsapiens.UCSC.hg19.knownGene")'
RUN R -e 'BiocManager::install("org.Hs.eg.db")'

# Added by Kai:
RUN R -e 'BiocManager::install("BSgenome.Mmusculus.UCSC.mm10")'
RUN R -e 'install.packages("tensorflow")'
RUN R -e 'install.packages("Matrix")'
RUN R -e 'BiocManager::install("TxDb.Mmusculus.UCSC.mm10.knownGene")'
RUN R -e 'install.packages("lattice")'
RUN R -e 'BiocManager::install("org.Mm.eg.db")'
RUN R -e 'install.packages("MASS")'
RUN R -e 'install.packages("testthat")'
# Below is for regenerating the Rd files with roxygen2:
RUN R -e 'install.packages("devtools")'
RUN R -e 'install.packages("roxygen2")'
# Below is to install Python tensorflow module: explicitly install miniconda first: image version 0.1
# Don't include tensorflow for version 0.2
RUN R -e 'reticulate::install_miniconda()'
RUN R -e 'tensorflow::install_tensorflow()'
# Below is to install CRISPRseek_1.32.5 for version 0.3
COPY CRISPRseek_1.35.2.tar.gz /tmp
RUN cd /tmp \
  && R -e 'install.packages("CRISPRseek_1.35.2.tar.gz", repo=NULL, type="source")'
