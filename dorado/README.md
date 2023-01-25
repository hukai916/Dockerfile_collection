# Docker image containing dorado-0.1.1

The dorado software is downloaded from https://cdn.oxfordnanoportal.com/software/analysis/dorado-0.1.1-linux-x64.tar.gz

## To use as Docker image:
```
docker pull hukai916/dorado:0.1
docker run -it hukai916/dorado:0.1
```

## To use as Singularity image:
```
singularity pull docker://hukai916/dorado:0.1
singularity shell dorado_0.1.sif
```
If on SCI (for UMass users), you can use the shared image, simply:
```
singularity shell /pi/mccb-umw/shared/singularity/dorado_0.1.sif
```
