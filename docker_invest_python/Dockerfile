FROM conda/miniconda3
# this is therealspring/invest-env:0.0.2
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
    ecoshard \
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
RUN hg pull && hg up 0ce0c93578b5
RUN conda run -v -n py37 python setup.py install

RUN hg clone https://richsharp@bitbucket.org/richsharp/invest /usr/local/invest
WORKDIR /usr/local/invest
RUN hg pull && hg up cbd062e0f549 # this is unstable/lingling-c-factor-exposure
RUN conda run -v -n py37 python setup.py install

RUN conda init bash
RUN echo "source activate py37" > ~/.bashrc
WORKDIR /workspace
CMD /bin/bash
