FROM conda/miniconda3
# this is therealspring/invest-env
USER root
RUN apt-get update \
&& apt-get install -y \
    build-essential \
    libspatialindex-c4v5 \
    mercurial \
&& rm -rf /var/lib/apt/lists/*

SHELL ["/bin/bash", "-c"]
RUN conda create -y --name py37 python=3.7 && conda clean -a -y
RUN conda run -v -n py37 conda install -c conda-forge gdal=2.4.1
RUN conda run -v -n py37 pip install --no-cache-dir \
    cython \
    ecoshard==0.3.1 \
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

RUN conda init bash && echo "source activate py37" > ~/.bashrc

RUN hg clone https://richsharp@bitbucket.org/richsharp/pygeoprocessing /usr/local/pygeoprocessing
WORKDIR /usr/local/pygeoprocessing
# this version of pygeoprocessing is the one with the C percentile implementation
RUN hg pull && hg up 0ce0c93578b5 && conda run -v -n py37 python setup.py install

RUN hg clone https://richsharp@bitbucket.org/richsharp/invest /usr/local/invest
WORKDIR /usr/local/invest
# this version of invest is unstable/SDR-using-d8
RUN hg pull && hg up 5d757e26166f && conda run -v -n py37 python setup.py install

WORKDIR /workspace
ENTRYPOINT ["/bin/bash", "-l", "-c"]
