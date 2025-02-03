FROM	continuumio/miniconda3

ADD environment_sci_work.yml /tmp/environment.yml

RUN conda env create -f /tmp/environment.yml
RUN apt-get update && apt-get install -y libgl1-mesa-glx 
# for opencv, imgaug

RUN echo "source activate $(head -1 /tmp/environment.yml | cut -d ' ' -f 2)" > ~/.bashrc
ENV PATH="/opt/conda/envs/$(head -1 /tmp/environment.yml | cut -d ' ' -f 2)/bin:$PATH"
# Build note: docker build --platform linux/amd64 -t hukai916/ccount_amd64:0.1 .