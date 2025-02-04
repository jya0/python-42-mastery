# syntax=docker/dockerfile:1
### It is important to put this syntax specification at the start!!
### https://docs.docker.com/build/dockerfile/frontend/
### https://stackoverflow.com/questions/75309371/dockerfile-with-heredoc-running-in-bash

### ARG and ENV declaration and initialisation
ARG UBUNTU_VER_LATEST=latest
ARG UBUNTU_VER_PENULTIMATE=jammy

### Base image
FROM ubuntu:${UBUNTU_VER_PENULTIMATE} AS base

### Installation phase

###### Utilities install
RUN <<EOF
apt update
apt install -y bash wget zsh
EOF

###### Setup oh-my-zsh (Zsh framework)
RUN <<EOF
bash -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
echo 'PROMPT=%B%F{blue}[DOCKER]%f%b$PROMPT' >> /root/.zshrc
EOF

### PYTHON 3.10 INSTALLATION using official Python 3.10.16 image
FROM python:3.10.16-bookworm AS python_build

###### Copy Python from the Python image to the base Ubuntu image
FROM base
COPY --from=python_build /usr/local /usr/local

###### Installation of additional Python Libraries
COPY ./requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

### Metadata
LABEL \
    author="jya0 <jyao@student.42abudhabi.ae>" \
    maintainer="jya0 <jyao@student.42abudhabi.ae>" \
    version="1.1" \
    description="This Dockerfile builds a dev environment to write and test python3.10 code."
