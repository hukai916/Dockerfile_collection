FROM hukai916/miniconda3_bio:0.1

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        python3-dev libhdf5-dev libfreetype6-dev libtool \
        m4 autoconf automake patch bison flex libpng-dev libopenblas-dev \
        tcl-dev tk-dev libxml2-dev zlib1g-dev libffi-dev cmake \
        apt-utils debconf locales && dpkg-reconfigure locales

RUN python -m venv /opt/venv
# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

# install dependencies:
COPY requirements_docker.txt /tmp/
RUN pip install --no-cache-dir --upgrade pip wheel && \
    pip install --no-cache-dir -r /tmp/requirements_docker.txt

# use version from argument (--build-arg version=0.11.0), or a default:
ARG version="0.11.2"
RUN pip install --no-cache-dir pyscenic==$version && \
    pip install --no-cache-dir scanpy==1.7.2

RUN apt-get -y update && \
    apt-get -y --no-install-recommends install \
        # Need to run ps
        procps \
        libxml2 \
        less \
        # Need to run MulticoreTSNE
        libgomp1 && \
    rm -rf /var/cache/apt/* && \
    rm -rf /var/lib/apt/lists/*

# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

RUN conda install jupyter
