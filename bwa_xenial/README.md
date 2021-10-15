# A docker image containing bwa installed with miniconda3 built on top of Xenial (Ubuntu-16.04)
**Docker image**: docker push hukai916/bwa_xenial:0.7.17

Notes:
1. There are many conflicts when installing multiqc v1.6 with conda, use pip install instead.
2. install bwa to base env so that you don't need to activate any env to access it.
