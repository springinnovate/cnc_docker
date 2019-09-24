FROM conda/miniconda3

# install build essentials & mercurial then delete the cache to minimize
# the docker size:
#   build-essential to build cython extensions
#   libspatialindex-c4v5 for the rtree python extension
#   mercurial for source control repo of PyGeoprocessing
USER root
RUN apt-get update \
&& apt-get install -y \
    build-essential \
    libspatialindex-c4v5 \
    mercurial \
&& rm -rf /var/lib/apt/lists/*

RUN /bin/bash -c "\
    conda create -y --name py37 python=3.7 \
    && conda clean -a -y"
RUN /bin/bash -c "conda run -v -n py37 conda install -c conda-forge gdal=2.4.1"
RUN conda run -v -n py37 pip install --no-cache-dir \
    cython \
    pytest \
    pytest-cov \
    mock \
    numpy \
    retrying \
    rtree \
    scipy \
    setuptools-scm \
    shapely \
    sympy \
    taskgraph && conda clean -a -y

RUN hg clone https://richsharp@bitbucket.org/richsharp/pygeoprocessing /usr/local/pygeoprocessing
WORKDIR /usr/local/pygeoprocessing
RUN hg pull && hg up 1.8.1rc1
RUN conda run -v -n py37 python setup.py install

RUN hg clone https://bitbucket.org/richsharp/invest /usr/local/invest
WORKDIR /usr/local/invest
RUN hg pull && hg up 1e1d4a64b599
RUN conda run -v -n py37 python setup.py install

WORKDIR /workspace
