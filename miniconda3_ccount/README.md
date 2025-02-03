# Docker/Singularity image for running `ccount`

**Note 1:** Since the `ccount` scripts include LSF jobs, the Docker image may not be suitable as Docker images are generally not compatible with LSF. While we have ensured that all necessary dependencies are installed in the Docker image, it is not yet useful in its current state until a local execution option is implemented.

**Note 2**: Other than the dependencies installed in the container, the `ccount` also relies on `ccount_utils/` and `pyimagesearch/`, which are included in the `workflow/` folder.

**Note 3**: The ccount training module relies on HPC LSF bsub commands to submit parallel jobs via Snakemake. Activating the container may overwrite the LSF commands, including `bsub`, which could cause Snakemake failures. For UMASS SCI users, the following command has been added into the `environment` variable in the image recipe, so it should just work. However, this solution is not guaranteed to work for all HPC systems as it depends on your HPC system setup, and you may need to consult with your HPC administrator for further assistance.  
```
source /lsf/conf/profile.lsf
```

## Docker:
```
# For ARM chips:
docker pull hukai816/ccount:0.1
docker run -it hukai816/ccount:0.1

# For AMD chips:
docker pull hukai816/ccount_amd64:0.1
docker run -it hukai816/ccount_amd64:0.1
```

## Singularity:
```
# First, download the ccount.sif by clicking the following link:
https://www.dropbox.com/scl/fi/z6749a9f3uvmi998r3ess/ccount.sif?rlkey=xy3nmej0mu70jghs86c8l92ur&dl=0

# Then activate an interactive session:
singularity shell ccount.sif

# (May be required depending on your LSF setup, inside the container)
source /lsf/conf/profile.lsf
```

## Dev notes:
```
export PATH=~/.conda/envs/ccount/bin/:$PATH
pip install aicspylibczi>=3.0.5
```