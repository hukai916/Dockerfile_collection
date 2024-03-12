# A Docker image containing DNAscent

## How to pull
```
docker pull hukai916/dnascent:4.0.1
singularity pull docker://hukai916/dnascent:4.0.1
```

## DNAscent GitHub repo: https://github.com/MBoemo/DNAscent

## Known building issues:
1. Seems that the v4.0.1 does not install well on M chips likely due to the hdf5 and tensorflow libraries.
2. Can be built with docker running ubuntu:22.04 on an Intel MacBook (2014), the following libraries need to be installed: liblzma-dev libbz2-dev libz-dev build-essential wget git
