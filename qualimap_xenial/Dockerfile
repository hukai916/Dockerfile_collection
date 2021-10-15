FROM hukai916/miniconda3_xenial:0.1

# Create conda env: conda_qualimap
COPY conda_qualimap.yaml .
RUN \
  conda env update -n base -f conda_qualimap.yaml \
  && conda clean -a

RUN apt-get update -q && apt-get install -y libfreetype6
