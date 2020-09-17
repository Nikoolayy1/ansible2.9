# ansible2.9


Ansible2.9:v1 uses Python 2 and and Ansible2.9:v2/latest uses Python 3. For working with Python3 on Ubuntu 18.04, you need to install Ansible with pip3, also pip3 does not add the directory /etc/ansible, so you need to manually create it and add files to it.


The DockerFile for Ansible2.9:v1 is:



FROM ubuntu:18.04
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Sofia
RUN apt update && apt install -y software-properties-common && \
    apt-add-repository --yes --update ppa:ansible/ansible && \
    apt install -y software-properties-common \
    vim \
    nano \
    ansible \
    python3-pip
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir paramiko && \
    pip3 install --no-cache-dir f5-sdk && \
    ansible-galaxy collection install f5networks.f5_modules
COPY ansible.cfg /etc/ansible/
COPY hosts /etc/ansible/




The DockerFile for Ansible2.9:v2/latest is:



FROM ubuntu:18.04
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Sofia
RUN apt update && apt install -y software-properties-common && \
    apt-add-repository --yes --update ppa:ansible/ansible && \
    apt install -y software-properties-common \
    vim \
    nano \
    python3-pip
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir ansible && \
    pip3 install --no-cache-dir paramiko && \
    pip3 install --no-cache-dir f5-sdk && \
    ansible-galaxy collection install f5networks.f5_modules
RUN mkdir /etc/ansible
RUN mkdir /etc/ansible/roles
COPY ansible.cfg /etc/ansible/
COPY hosts /etc/ansible/




I used Ubuntu 18.04, because there is a bug when trying to install Ansible on 20.04. The Ansible version is 2.9 and I also installed paramiko for managing Cisco and some F5 Big-IP modules. I added some config to the hosts file, so that you can test the container as it will connect to the Cisco dCloud free lab router. The test command is:

ansible routers -m ios_command -a "commands='show ip int brief'"
