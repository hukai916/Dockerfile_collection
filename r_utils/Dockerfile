FROM ubuntu:16.04

# essential tools
RUN apt-get update && apt-get install -y --no-install-recommends \
	poppler-utils \
	imagemagick \
		# above 2 required for converting pdf to jpeg and merging jpeg
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
  r-base=4.1.2-1.1604.0 \
  r-recommended=4.1.2-1.1604.0 \
  r-base-core=4.1.2-1.1604.0 \
  r-base-dev=4.1.2-1.1604.0 \
  && ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*

# install SAMtools:
COPY samtools-1.14.tar.bz2 ./
RUN tar -xf samtools-1.14.tar.bz2
RUN cd samtools-1.14 && ./configure && make && make install

# install essential packages for installing desired R packages:
RUN apt-get update && apt-get install -y --no-install-recommends \
	build-essential \
	libcurl4-gnutls-dev \
	libxml2-dev \
	libssl-dev \
# above are required by R::devtools, ref: https://github.com/r-lib/devtools/issues/2131
	libjpeg-turbo8-dev \
	libpng-dev \
	libbz2-dev \
	liblzma-dev \
# above are required by ShortRead
	gawk
# above required to use gensub

RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
# required for installation of curl -> campfin; also required for ShortRead (may be redundant to libcurl4-gnutls-dev)

# install R utilities:
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'install.packages("devtools")'
RUN R -e 'install.packages("collections")'
RUN R -e 'install.packages("optparse")'
RUN R -e 'install.packages("stringr")'
RUN R -e 'install.packages("purrr")'
RUN R -e 'install.packages("dplyr")'
RUN R -e 'install.packages("campfin")'

RUN R -e 'BiocManager::install("GenomicFeatures")'
RUN R -e 'BiocManager::install("ShortRead")'
RUN R -e 'BiocManager::install("BSgenome")'
RUN R -e 'BiocManager::install("biomaRt")'
RUN R -e 'BiocManager::install("plyranges")'
RUN R -e 'BiocManager::install("GenomeInfoDb")'
