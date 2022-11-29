# A docker image containing software tools for nanopore data analysis based on Xenial (Ubuntu-16.04)


## Tools installed: v0.1
1.  DNAscent: https://github.com/MBoemo/DNAscent/tree/3.1.2
2.  bonito: https://github.com/nanoporetech/bonito


## How to use:
### With Docker on Mac (interactive mode):
```
docker run -it -v /Users:/Users hukai916/nanopore_util:0.1
```

### With Singularity on SCI (interactive mode):
```
singularity pull docker://hukai916/nanopore_util:0.1
singularity shell nanopore_util_0.1.sif
```
Or use the centralized image:
```
singularity shell /pi/mccb-umw/shared/singularity/nanopore_util_0.1.sif
```
