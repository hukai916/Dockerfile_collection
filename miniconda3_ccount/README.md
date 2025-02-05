# Docker image for running `ccount`

**Note 1**: Other than the dependencies installed in the container, the `ccount` also relies on `ccount_utils/` and `pyimagesearch/`, which are included in the `workflow/` folder.

## Docker:
```
# With tensorflow:
docker pull hukai916/ccount_cpu:0.1

# With tensorflow-gpu:
docker pull hukai916/ccount_gpu:0.1

# Run on AMD chip:
docker run -it hukai916/ccount_cpu:0.1
docker run -it hukai916/ccount_gpu:0.1

# Run on ARM chip (Apple M-chip):
docker run --platform linux/amd64 -it hukai916/ccount_cpu:0.1
docker run --platform linux/amd64 -it hukai916/ccount_gpu:0.1
```

## Dev notes:
1. `environment_cpu.yml`: yaml used to build conda env on SCI with tensorflow
2. `environment_gpu.yml`: yaml used to build conda env on SCI with tensorflow-gpu
3. `environment_cpu_sci_work.yml`: yaml exported from the SCI env built with 1)
4. `environment_gpu_sci_work.yml`: yaml exported from the SCI env built with 2)

```
export PATH=~/.conda/envs/ccount/bin/:$PATH
pip install aicspylibczi>=3.0.5
```