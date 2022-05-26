FROM ubuntu:16.04

# essential tools
RUN apt-get update && apt-get install -y --no-install-recommends \
	build-essential
		# above is required by R::devtools and MACS2 (gcc)

# other utility tools
RUN apt-get update && apt-get install -y --no-install-recommends \
	less \
	wget \
	git

# install base R: adapted from https://github.com/rocker-org/rocker/blob/master/r-ubuntu/focal/Dockerfile
	# step1: preparation
RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker \
	&& addgroup docker staff
RUN apt-get update && apt-get install -y --no-install-recommends \
	software-properties-common \
  dirmngr \
  ed \
	locales \
	vim-tiny \
	ca-certificates \
	&& add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0" \
  && add-apt-repository --enable-source --yes "ppa:c2d4u.team/c2d4u4.0+"
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV TZ UTC

	# step2: install specific version of R
RUN apt-get update && apt-get install -y --no-install-recommends apt-transport-https
# above is required for fetching r sources
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  littler \
  r-base=4.2.0-1.1604.0 \
  r-recommended=4.2.0-1.1604.0 \
  r-base-core=4.2.0-1.1604.0 \
  r-base-dev=4.2.0-1.1604.0 \
  && ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*

# install utility tools for building, checking, and running R packages:
	# step1: preparation
RUN apt-get update && apt-get install -y --no-install-recommends \
	libxt-dev \
	libcairo2-dev	\
		# above 2 are required by ArchR
	build-essential \
	libcurl4-gnutls-dev \
	libxml2-dev \
	libssl-dev \
		# above are required by R::devtools, ref: https://github.com/r-lib/devtools/issues/2131
	libjpeg-turbo8-dev \
	libpng-dev \
	libbz2-dev \
	liblzma-dev
		# above are required by ShortRead
RUN R -e 'install.packages("BiocManager")'

# For building, checking, and running GUIDEseq and CRISPRseek:
RUN R -e 'install.packages("knitr")'
RUN R -e 'BiocManager::install("ChIPpeakAnno")'
RUN R -e 'BiocManager::install("CRISPRseek")'
RUN R -e 'BiocManager::install("limma")'
RUN R -e 'BiocManager::install("BiocStyle")'
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg19")'
RUN R -e 'BiocManager::install("TxDb.Hsapiens.UCSC.hg19.knownGene")'
RUN R -e 'BiocManager::install("org.Hs.eg.db")'
RUN R -e 'install.packages("RUnit")'
RUN R -e 'install.packages("testthat")'

RUN R -e 'tinytex::install_tinytex()'
RUN apt install -y texlive-fonts-extra
# RUN apt-get update \
#  && apt-get install -y --no-install-recommends texlive-fonts-extra
# Above two is for making pdflatex available. Ref: https://stackoverflow.com/questions/14333759/diagnosing-r-package-build-error-pdflatex-not-available

# for R CMD check CRISPRseekxxx:
RUN R -e 'BiocManager::install("BSgenome.Mmusculus.UCSC.mm10")'
RUN R -e 'BiocManager::install("TxDb.Mmusculus.UCSC.mm10.knownGene")'
RUN R -e 'BiocManager::install("org.Mm.eg.db")'

# for pdflatex to be installed correctly (reason unknown)
RUN apt purge -y texlive-fonts-extra && apt install -y texlive-fonts-extra

# for testing only: v0.2
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg38")'
RUN R -e 'BiocManager::install("TxDb.Hsapiens.UCSC.hg38.knownGene")'
RUN R -e 'install.packages("remotes")'

# for rendering .Rmd with rmarkdown to pdf: v0.3
RUN apt purge -y texlive-fonts-extra && apt install -y pandoc pandoc-citeproc texlive-extra-utils texlive-fonts-extra
