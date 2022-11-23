## build a base miniconda3 image based on Xenial
## adapted from: https://github.com/ContinuumIO/docker-images/blob/master/miniconda3/debian/Dockerfile

FROM ubuntu:16.04

LABEL maintainer="Kai Hu@UMMS"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# hadolint ignore=DL3008
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends \
        bzip2 \
        ca-certificates \
        git \
        libglib2.0-0 \
        libsm6 \
        libxext6 \
        libxrender1 \
        mercurial \
        subversion \
        wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV PATH /opt/conda/bin:$PATH

CMD [ "/bin/bash" ]

# Leave these args here to better use the Docker build cache
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

# add curl
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends \
            curl

# add Python Bio package:
RUN conda install -c conda-forge biopython

# add bs4 package:
RUN conda install bs4

# add gawk:
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends \
    gawk

# add gffread:
RUN conda install -c bioconda gffread

# add bioinfokit:
RUN pip install bioinfokit==2.0.1

# add bgzip:
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends tabix

# add pysam:
RUN conda install -c bioconda pysam

# add regex:
RUN conda install regex

# update numpy:
RUN conda install numpy

# add dos2unix:
RUN conda install -c conda-forge dos2unix
