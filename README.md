# ansible2.9

Dockerfile (ubuntu 20.04 has a bug, when trying to install Ansible)

FROM ubuntu:18.04
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Sofia
RUN apt update && apt install -y software-properties-common && \
    apt-add-repository --yes --update ppa:ansible/ansible && \
    apt install -y software-properties-common \
    ansible \
    python3-pip
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir paramiko && \
    pip3 install --no-cache-dir f5-sdk && \
    ansible-galaxy collection install f5networks.f5_modules
