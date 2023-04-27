FROM continuumio/miniconda3:22.11.1

# install required dependencies
RUN apt-get update -q && apt-get install -y build-essential

# v0.1
RUN conda install -c bioconda minimap2
COPY QAlign /QAlign
RUN cd QAlign && python setup.py install

