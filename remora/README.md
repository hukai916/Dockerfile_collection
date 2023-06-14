# Docker image containing remora

## v0.1
- remora-2.0.0

## v0.2 
- remora-2.1.2

## To use as Docker image:
```
docker pull hukai916/remora:0.2
docker run -it hukai916/remora:0.2
```

## To use as Singularity image:
```
singularity pull docker://hukai916/remora:0.2
singularity shell remora_0.2.sif
```
If on SCI (for UMass users), you can use the shared image, simply:
```
singularity shell /pi/mccb-umw/shared/singularity/remora_0.2.sif
```
