FROM hukai916/miniconda3_xenial:0.1

RUN conda install -c bioconda -c conda-forge multiqc

# install MultiQC plugins:
RUN git clone https://github.com/hukai916/multiqc_plugins.git
RUN cd multiqc_plugins/archr_plugin && python setup.py install
RUN cd multiqc_plugins/barcode_correction_plugin && python setup.py install
RUN cd multiqc_plugins/remove_duplicate_plugin && python setup.py install
RUN cd multiqc_plugins/bam_filter_plugin && python setup.py install
