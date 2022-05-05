# A Docker image built for testing GUIDEseq and CRISPRseek with R 4.2.0.
## Based on:
Ubuntu-16.04
## How to download:
docker pull hukai916/r_4.2.0:0.1
## base R: 4.2.0

## Building notes:
1.  All packages required to run "R CMD check/build GUIDEseq/CRISPRseek" have been installed with this image build.
2.  The pdflatex is required to run "R CMD build", it is tricky to install though. The following works: note that you likely need to remove and purge pre-existing texlive-fonts-extra first

RUN R -e 'tinytex::install_tinytex()' # may not be a must

RUN apt purge -y texlive-fonts-extra && apt install -y texlive-fonts-extra
