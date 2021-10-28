FROM ubuntu:16.04

## Set a default user. Available via runtime flag `--user docker`
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly).
RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker \
	&& addgroup docker staff

## Install R 4.1.1:
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
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  littler \
  gawk \
  r-base=4.1.1-1.1604.0 \
  r-recommended=4.1.1-1.1604.0 \
  r-base-core=4.1.1-1.1604.0 \
  r-base-dev=4.1.1-1.1604.0 \
  && ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*

## Install other essentials:
RUN apt-get update && apt-get install -y --no-install-recommends git
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    libxml2-dev \
    libssl-dev \
    pandoc \
    libcurl4-openssl-dev # required for installation of curl -> campfin; also required for ShortRead

## Install miniconda:
ARG CONDA_VERSION=py38_4.9.2
ARG CONDA_MD5=122c8c9beb51e124ab32a0fa6426c656

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh -O miniconda.sh && \
    echo "${CONDA_MD5}  miniconda.sh" > miniconda.md5 && \
    if ! md5sum --status -c miniconda.md5; then exit 1; fi && \
    mkdir -p /opt && \
    sh miniconda.sh -b -p /opt/conda && \
    rm miniconda.sh miniconda.md5 && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

ENV PATH /opt/conda/bin:$PATH

# Install rhdf5
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'BiocManager::install("rhdf5")'

# Install hdf5 lib
RUN apt-get update && apt-get install -y --no-install-recommends \
  libhdf5-serial-dev hdf5-tools

# Install ont_fast5_api
RUN pip install ont-fast5-api

# Install vbz_compression
ADD ont-vbz-hdf-plugin_1.0.1-1.xenial_amd64.deb ./
RUN dpkg -i ont-vbz-hdf-plugin_1.0.1-1.xenial_amd64.deb
