# Copyright 2017 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# -----------------------------------------------------------------------------

# This Dockerfile is for running protogen, lint, and unit-tests

FROM ubuntu:xenial

RUN echo "deb apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8AA7AF1F1091A5FD" && \
    echo 'deb http://repo.sawtooth.me/ubuntu/1.0/stable xenial universe' >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y --allow-unauthenticated -q \
        python3-pip \
        python3-sawtooth-sdk \
        python3-sawtooth-rest-api \
        python3-sawtooth-cli \
        python3-sawtooth-signing \
        cron-apt \
        curl \
        software-properties-common \
        wget \
        zlib1g-dev \
        zlibc \
        build-essential \
        checkinstall \
        libffi-dev

# need ssl
RUN wget https://www.openssl.org/source/openssl-1.1.1s.tar.gz
RUN tar -xf openssl-1.1.1s.tar.gz
WORKDIR openssl-1.1.1s
RUN ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl shared zlib
RUN make && make test && make install
RUN echo /usr/local/ssl/lib >> /etc/ld.so.conf.d/openssl-1.1.1s.conf
RUN ldconfig -v

# neeed newer version of python
RUN wget https://www.python.org/ftp/python/3.7.17/Python-3.7.17.tar.xz
RUN tar -xf Python-3.7.17.tar.xz
WORKDIR Python-3.7.17
RUN ./configure --enable-optimizations
RUN make altinstall

RUN pip3.7 install --upgrade pip


RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
    && apt-get install -y nodejs

RUN pip3.7 install \
    pylint==1.8.1 \
    pycodestyle==2.3.1 \
    grpcio-tools==1.7.0 \
    nose2==0.7.2 \
    bcrypt \
    pycrypto \
    rethinkdb \
    sanic \
    itsdangerous

WORKDIR /project/sawtooth-marketplace

COPY sawbuck_app/package.json /project/sawtooth-marketplace/sawbuck_app/

RUN cd sawbuck_app/ && npm install

ENV PATH $PATH:/project/sawtooth-marketplace/bin
