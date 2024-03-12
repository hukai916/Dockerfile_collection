FROM ubuntu:22.04

# install DNAscent
RUN apt-get update && apt-get install build-essential wget git -y
RUN apt-get update && apt-get install liblzma-dev libbz2-dev libz-dev -y

COPY DNAscent /DNAscent
RUN cd DNAscent && make
ENV PATH="${PATH}:/DNAscent/bin"