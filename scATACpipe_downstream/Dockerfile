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

# install MACS2 via Miniconda3
ENV PATH /opt/conda/bin:$PATH
CMD [ "/bin/bash" ]
ARG CONDA_VERSION=py38_4.9.2
ARG CONDA_MD5=122c8c9beb51e124ab32a0fa6426c656
RUN wget --quiet --no-check-certificate https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh -O miniconda.sh && \
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
RUN pip install MACS2

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

# install ArchR (Haibo's repo)
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
RUN R -e 'install.packages("optparse")'
RUN R -e 'install.packages("pheatmap")' # required by ArchR heatmap plotting
RUN R -e 'BiocManager::install("scran")' # required by ArchR clustering with scran
RUN R -e 'install.packages("hexbin")' # required by ArchR plotting
RUN R -e 'install.packages("collections")'
RUN apt-get update && apt-get install -y --no-install-recommends libhdf5-dev
	# libhdf5-dev required by hdf5r
RUN R -e 'install.packages("hdf5r")' # required by Seurat::Read10X_h5
RUN R -e 'install.packages("Seurat")'
RUN R -e 'install.packages("hdf5r")' # required by Seurat::Read10X_h5
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg19")' # required by ArchR native supported genomes
RUN R -e 'BiocManager::install("BSgenome.Hsapiens.UCSC.hg38")' # required by ArchR native supported genomes
RUN R -e 'BiocManager::install("BSgenome.Mmusculus.UCSC.mm10")' # required by ArchR native supported genomes
RUN R -e 'BiocManager::install("BSgenome.Mmusculus.UCSC.mm9")' # required by ArchR native supported genomes
RUN R -e 'install.packages("devtools")'
RUN apt-get update && apt-get install -y --no-install-recommends libgsl-dev
	# libgsl-dev required by TFBSTools
RUN R -e 'BiocManager::install("TFBSTools")' # required by chromVAR
RUN R -e 'BiocManager::install("chromVAR")' # required by ArchR
RUN R -e 'devtools::install_github("GreenleafLab/chromVARmotifs")' # required by ArchR: motif deviation

	# step2: install ArchR from Haibo's repo:
RUN R -e 'install.packages("future")'
RUN R -e 'install.packages("Cairo")' # required by ArchR
# RUN R -e 'devtools::install_github("haibol2016/ArchR", ref="master", repos = BiocManager::repositories())'
RUN R -e 'devtools::install_github("hukai916/ArchR-1", ref="master", repos = BiocManager::repositories())'
RUN R -e 'library(ArchR);ArchR::installExtraPackages()'
