FROM hukai916/miniconda3_xenial:0.1

# Create conda env: conda_bwa
COPY conda_bwa.yaml .
RUN \
  conda env update -n base -f conda_bwa.yaml \
  && conda clean -a
