FROM ubuntu:22.04

# install DNAscent
RUN apt-get update && apt-get install build-essential wget git -y
RUN apt-get update && apt-get install liblzma-dev libbz2-dev libz-dev -y

RUN git clone --recursive https://github.com/MBoemo/DNAscent.git
RUN cd DNAscent && git checkout 4.0.1 && make

ENV PATH="${PATH}:/DNAscent/bin"