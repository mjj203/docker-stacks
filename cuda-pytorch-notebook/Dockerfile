# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG OWNER=jupyter
ARG BASE_CONTAINER=$OWNER/cuda-base-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Michael Jones <40840725+mjj203@users.noreply.github.com>"

# Install python dependencies for torch and torchvision compatible with CUDA 11.7.1
RUN python -m pip install -U --no-cache-dir \
      setuptools pip && \
	FORCE_CUDA=1 pip install --no-cache-dir \
	  cPython \
	# Useful application for model performance tracking https://docs.wandb.ai/
	  wandb \
	  cupy-cuda117 && \
	# Common data science libraries
	mamba install -c \
	  conda-forge --quiet --yes \
	  Cython numba \
	  sqlalchemy statsmodels \
	  psycopg2-binary numpy \
	  ninja pyyaml cffi boto3 \
	  botocore pandas && \
	# linear algebra (LA) GPU accelerated libraries
	FORCE_CUDA=1 mamba install -c \
	  pytorch -c conda-forge -c nvidia -c intel \
	  --quiet --yes magma-cuda117 && \
	# torch and torchvision compatible with CUDA 11.7.x
	FORCE_CUDA=1 pip install --no-cache-dir --pre torch \
	  torchvision torchaudio \
	  --extra-index-url https://download.pytorch.org/whl/nightly/cu117 && \
	mamba clean --all -f -y && \
	jupyter lab clean && \
    jupyter lab build
