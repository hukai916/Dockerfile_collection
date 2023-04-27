# Docker image containing:
### minicond3
### qalign
### minimap2

## To use as a Docker container:
```
docker pull hukai916/qalign:0.1
```
Then, run it interactively:
```
docker run -it hukai916/qalign:0.1
```

Or non-interactively:
```
docker run hukai916/qalign:0.1 python QAlign/qalign/main.py convert --input_fasta QAlign/qalign/test_samples/reads.fasta --outdir QAlign/qalign/test_samples/output/ --qlevel 2 --rc 1 --kmerpath QAlign/qalign/kmermap_models/r9.4_6mer_nucleotide_template_model.txt
```

## To use as a Singularity container:
```
singularity pull docker://hukai916/qalign:0.1
```
Then, run it interactively:
```
singularity shell qalign_0.1.sif
```
- Note that the `QAlign` folder is under the `./` directory.

Or non-interactively:
```
singularity exec qalign_0.1.sif python /QAlign/qalign/main.py convert --input_fasta /QAlign/qalign/test_samples/reads.fasta --outdir ./output/ --qlevel 2 --rc 1 --kmerpath /QAlign/qalign/kmermap_models/r9.4_6mer_nucleotide_template_model.txt
```
- Note that the `/` in front of the `QAlign/` and the `--outdir` must point to a location that you have write access to.