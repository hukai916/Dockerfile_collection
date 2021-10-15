FROM ubuntu:16.04

COPY cellranger-atac-2.0.0.tar.gz /opt

RUN cd /opt && tar -zxvf cellranger-atac-2.0.0.tar.gz

ENV PATH="/opt/cellranger-atac-2.0.0:${PATH}"
