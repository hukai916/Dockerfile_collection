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
ARG CONDA_VERSION=py39_4.9.2
ARG CONDA_MD5=b4e46fcc8029e2cfa731b788f25b1d36

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

# Version 0.2: add curl
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends \
            curl

RUN pip install cutadapt==3.4
# conda install won't work for some reason

# RUN python3 -m pip install --user --upgrade cutadapt
# RUN mv /root/.local/bin/* /bin/

# RUN python3 -m pip install --user --upgrade cutadapt
# ENV PATH=/root/.local/bin:$PATH
# above two methods won't work when deploying in hpcc.

# for cutadapt to output gzipped fastq files in parallel:
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends \
            pigz
