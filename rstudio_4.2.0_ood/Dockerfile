FROM centos:centos7

ENV PATH=/usr/lib/rstudio-server/bin:${PATH}

RUN yum -y update && yum -y install wget # This is a good place to add additional yum packages you need installed


#    Installing R per https://github.com/rstudio/r-builds
#    Define the version of R that you want to install. Available versions of R can be found here: https://cdn.rstudio.com/r/versions.json

#export R_VERSION=4.0.5
ARG R_VERSION=4.2.0

#    Enable EPEL

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

#    Install R

RUN wget https://cdn.rstudio.com/r/centos-7/pkgs/R-${R_VERSION}-1-1.x86_64.rpm
RUN yum -y install R-${R_VERSION}-1-1.x86_64.rpm

RUN ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R
RUN ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript

#    Install Rstudio Server
#    Please note: newer version of rstudio are failing to work with OOD, these are commented for future reference, but do not uncomment and use newer versions.

#wget https://download2.rstudio.org/server/centos7/x86_64/rstudio-server-rhel-2022.02.0-443-x86_64.rpm
#wget https://s3.amazonaws.com/rstudio-ide-build/server/centos7/x86_64/rstudio-server-rhel-2021.09.2-382-x86_64.rpm
#wget https://s3.amazonaws.com/rstudio-ide-build/server/centos7/x86_64/rstudio-server-rhel-2021.09.1-372-x86_64.rpm
#wget https://download2.rstudio.org/server/centos7/x86_64/rstudio-server-rhel-2021.09.0-351-x86_64.rpm
RUN wget https://download2.rstudio.org/server/centos6/x86_64/rstudio-server-rhel-1.3.1093-x86_64.rpm

#yum -y install rstudio-server-rhel-2022.02.0-443-x86_64.rpm
#yum -y install rstudio-server-rhel-2021.09.2-382-x86_64.rpm
#yum -y install rstudio-server-rhel-2021.09.1-372-x86_64.rpm
#yum -y install rstudio-server-rhel-2021.09.0-351-x86_64.rpm
RUN yum -y install rstudio-server-rhel-1.3.1093-x86_64.rpm

# EXAMPLE R PACKAGE INSTALLS:

# Install leidenbase and monocle3 using devtools::install_github
#echo "devtools::install_github('cole-trapnell-lab/leidenbase')" | Rscript -
#echo "devtools::install_github('cole-trapnell-lab/monocle3')" | Rscript -

# Install bioconductor packages - there may be better ways to do this, but this is working currently.
#echo 'install.packages("BiocManager")
#BiocManager::install(version = "3.12")' | Rscript -
#echo "BiocManager::install(c('BiocGenerics', 'DelayedArray', 'DelayedMatrixStats',
#                   'limma', 'S4Vectors', 'SingleCellExperiment',
#                   'SummarizedExperiment', 'batchelor', 'Matrix.utils'))" | Rscript -

# Install DiffBind
#echo 'if (!requireNamespace("BiocManager", quietly = TRUE))
#install.packages("BiocManager")
#BiocManager::install("DiffBind")' | Rscript -

# Clean up
RUN rm -rf /var/run && ln -s /tmp /var/run

# install missing libraries:
RUN yum -y install openssl-devel
RUN yum -y install libpng-devel
RUN yum -y install fontconfig-devel
RUN yum -y install freetype-devel
RUN yum -y install centos-release-scl
RUN yum -y install devtoolset-7-gcc*
RUN scl enable devtoolset-7 bash
RUN yum -y install geos-devel
