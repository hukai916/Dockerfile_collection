FROM continuumio/miniconda3:23.3.1-0

RUN conda install -c bioconda samtools

RUN conda install -c conda-forge biopython

RUN conda install -c conda-forge regex

RUN conda install -c conda-forge matplotlib

RUN conda install -c anaconda pandas

RUN conda install -c bioconda bwa

# v0.3: umi_tools can't be installed properly
# RUN pip install umi_tools

# v0.3
RUN pip install scutls==0.2.6

# v0.4 
RUN conda install -c bioconda minimap2

# v0.5
RUN pip install scutls==0.2.7

# v0.6 
RUN apt-get update -q && apt-get install -y bc