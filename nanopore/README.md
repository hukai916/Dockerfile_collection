# A docker image containing software tools for nanopore data analysis based on Xenial (Ubuntu-16.04)

## Tools installed:
R 4.1.1 <br>
miniconda3 <br>
R package: rhdf5 <br>
libhdf5-serial-dev <br>
hdf5-tools <br>
Python tool: ont-fast5-api <br>
ont-vbz-hdf-plugin_1.0.1 <br>

## How to use:
### With Docker on Mac (interactive mode):
```
docker run -it -v /Users:/Users hukai916/nanopore:0.1
```

### With Singularity on HPCC (interactive mode):
```
singularity pull docker://hukai916/nanopore:0.1
singularity shell nanopore_0.1.sif
```
