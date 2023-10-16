FROM nvidia/cuda:11.2.2-cudnn8-runtime-ubuntu20.04

USER root

RUN apt-get update \
	&& apt-get install -y wget

# python
RUN apt install -y python3
RUN apt install -y python3-pip
RUN pip3 install --upgrade pip

RUN apt-get update
RUN apt-get install -y python3-dev
RUN apt-get install -y git

# jupyter
RUN pip3 install jupyter
RUN pip3 install nbdev

# standard libraries
RUN pip3 install scipy
RUN pip3 install seaborn
RUN pip3 install regex

# astrodynamics libraries
RUN pip3 install TLE-tools
RUN pip3 install pykep
RUN pip3 install spiceypy

# machine learning libraries
RUN pip3 install numba
RUN pip3 install scikit-learn
RUN pip3 install xgboost
RUN pip3 install umap-learn
RUN pip3 install --upgrade wandb

# torch import
#RUN pip3 install torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
#RUN pip3 install torch
RUN pip3 install numpy --pre torch --force-reinstall --index-url https://download.pytorch.org/whl/nightly/cu117

# import fastai2 stable track
RUN pip3 install fastai==2.1.7
RUN pip3 install fastcore==1.3.10

# hdbscan (bug dec 2022)
RUN pip3 install joblib==1.1.0
RUN pip3 install hdbscan

# set environment
# needed for weights and biases (shouldn't need for >python3.7)
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

# Time series extra utils for data loaders
RUN pip3 install git+https://github.com/vrodriguezf/timeseries.git
RUN pip3 install --upgrade git+https://github.com/timeseriesAI/tsai.git

RUN ln -s /usr/local/cuda-10.0 /usr/local/nvidia

# user for jupyter
ARG USER_ID=1000
ARG GROUP_ID=1000
ARG USER=user

RUN addgroup --gid $GROUP_ID $USER
RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID $USER

# Install Jupyter extensions
#RUN pip3 install jupyter_contrib_nbextensions
#RUN jupyter contrib nbextension install --user
#RUN jupyter nbextensions_configurator enable --user
#RUN jupyter nbextension enable collapsible_headings/main --user

# MAKE DEAFULT CONFIG
RUN jupyter notebook --generate-config
RUN mkdir host_data

USER $USER
WORKDIR /home/$USER

# run jupyter notebook automatically [https://www.kubeflow.org/docs/notebooks/custom-notebook/]
CMD ["sh","-c","jupyter notebook --ip 0.0.0.0 --no-browser --allow-root"]
