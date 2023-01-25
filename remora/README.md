# Docker image containing remora-2.0.0

## To use as Docker image:
```
docker pull hukai916/remora:0.1
docker run -it hukai916/remora:0.1
```

## To use as Singularity image:
```
singularity pull docker://hukai916/remora:0.1
singularity shell remora_0.1.sif
```
If on SCI (for UMass users), you can use the shared image, simply:
```
singularity shell /pi/mccb-umw/shared/singularity/remora_0.1.sif
```
