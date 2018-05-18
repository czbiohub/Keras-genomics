FROM haoyangz/keras-docker:cuda8.0
MAINTAINER Haoyang Zeng  <haoyangz@mit.edu>

RUN apt-get update

# download and install
RUN curl https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh > /tmp/miniconda.sh
RUN bash /tmp/miniconda.sh -b -p $HOME/anaconda

RUN export PATH=$HOME/anaconda/bin:$PATH # add to PATH
RUN echo 'export PATH=$HOME/anaconda/bin:$PATH' >> ~/.bashrc # add to bashrc for future use
RUN . ~/.bashrc
RUN hash -r

# some configuration to make it easy to install things
RUN conda config --set always_yes yes --set changeps1 no
RUN conda update -q conda

# add channels to look for packages
RUN conda config --add channels r # for backward compatibility with old r packages
RUN conda config --add channels defaults
RUN conda config --add channels conda-forge # additional common tools
RUN conda config --add channels bioconda # useful bioinformatics

RUN conda install -n root _license

# display info
RUN conda info -a

RUN pip install --upgrade pip
RUN pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas@0.2
RUN pip install --upgrade --no-deps git+https://github.com/hyperopt/hyperopt.git
RUN pip install pymongo scikit-learn networkx jupyter pandas

ENV THEANO_FLAGS='cuda.root=/usr/local/cuda,device=gpu0,floatX=float32,lib.cnmem=0.1,base_compiledir=/runtheano/.theano'

RUN git clone https://github.com/gifford-lab/keras-genomics.git /scripts/keras-genomics
RUN cp /scripts/keras-genomics/main.py /scripts/
RUN mkdir /runtheano/;chmod -R 777 /runtheano/
WORKDIR /scripts/
