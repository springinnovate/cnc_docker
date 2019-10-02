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
RUN hg pull && hg up 23a1ea7b13b0 && conda run -v -n py37 python setup.py install

RUN hg clone https://richsharp@bitbucket.org/richsharp/invest /usr/local/invest
WORKDIR /usr/local/invest
# this version of invest is unstable/lingling-c-factor-exposure
RUN hg pull && hg up f987abed03be && conda run -v -n py37 python setup.py install

RUN conda init bash && echo "source activate py37" > ~/.bashrc
WORKDIR /workspace
CMD /bin/bash
