FROM ubuntu:18.04

RUN apt-get update && apt-get install -y build-essential git

RUN git clone https://github.com/Roy-lab/scMTNI.git

RUN apt-get update && apt-get install libgsl-dev -y
RUN cd scMTNI/Code/ && make

RUN echo 'alias scMTNI="/scMTNI/Code/scMTNI"' >> /root/.bashrc