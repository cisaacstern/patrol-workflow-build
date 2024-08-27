FROM mambaorg/micromamba:latest as build

ARG ENVFILE=environment.yml
ARG ENVNAME=lithops-worker

USER root
RUN apt-get update && apt-get install -y \
        zip \
        && rm -rf /var/lib/apt/lists/*
USER $MAMBA_USER

# build the user-defined worker environment, which also adds python + pip
COPY $ENVFILE ./
RUN micromamba create --file $ENVFILE --name $ENVNAME --yes \
    && micromamba clean --all --yes

# install the lithops dependencies into the new environment
RUN micromamba run -n $ENVNAME python -m pip install --upgrade setuptools six pip \
    && micromamba run -n $ENVNAME python -m pip install --no-cache-dir \
        wheel \
        gunicorn \
        pika \
        flask \
        gevent \
        redis \
        requests \
        PyYAML \
        kubernetes \
        numpy \
        cloudpickle \
        ps-mem \
        tblib \
        namegenerator \
        cryptography \
        httplib2 \
        google-cloud-storage \
        google-api-python-client \
        gcsfs \
        google-auth \
        psutil

ENV CONCURRENCY 1
ENV TIMEOUT 600

# Copy Lithops proxy and lib to the container image.
# ENV APP_HOME /lithops
# WORKDIR $APP_HOME

# COPY lithops_cloudrun.zip .
# RUN unzip lithops_cloudrun.zip && rm lithops_cloudrun.zip

# CMD exec gunicorn --bind :$PORT --workers $CONCURRENCY --timeout $TIMEOUT lithopsproxy:proxy
