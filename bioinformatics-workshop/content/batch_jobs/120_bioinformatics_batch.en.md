---
title: "Dockerfiles for AWS Batch"
weight: 120
---

AWS Batch enables developers, scientists, and engineers to easily and 
efficiently run hundreds of thousands of batch computing jobs on AWS. AWS Batch 
dynamically provisions the optimal quantity and type of compute resources 
(e.g., CPU or memory optimized instances) based on the volume and specific 
resource requirements of the batch jobs submitted.

AWS Batch jobs are based on docker containers which means we will have to 
containerize the bioinformatics tools we want to run and also any custom code 
we want to add to the pipeline.


## Bioinformatics public docker containers

You can find many bioinformatics docker containers on the web without the need 
to author them from scratch. For example 
(https://github.com/StaPH-B/docker-builds) has many bioinformatics Dockerfiles 
(A Dockerfile is a text file that describe the information needed to build a 
docker container).

An example Dockerfile for minimap2 taken from 
(https://github.com/StaPH-B/docker-builds/blob/master/minimap2/2.17/Dockerfile)
```
FROM ubuntu:xenial

# metadata
LABEL base.image="ubuntu:xenial"
LABEL container.version="1"
LABEL software="Minimap2"
LABEL software.version="2.17"
LABEL description="versatile sequence alignment program that aligns DNA or mRNA sequences against a large reference database"
LABEL website="https://github.com/lh3/minimap2"
LABEL license="https://github.com/lh3/minimap2/blob/master/LICENSE.txt"
LABEL maintainer="Kelsey Florek"
LABEL maintainer.email="Kelsey.florek@slh.wisc.edu"

RUN apt-get update && apt-get install -y python \
  curl\
  bzip2

RUN curl -L https://github.com/lh3/minimap2/releases/download/v2.17/minimap2-2.17_x64-linux.tar.bz2 | tar -jxvf -

ENV PATH="${PATH}:/minimap2-2.17_x64-linux"
WORKDIR /data
```


## Creating your own Dockerfile

There are many tutorials about docker containers on the web, for example 
[this one from docker.com](https://docs.docker.com/get-started/02_our_app/).

Here’s an example for a Dockerfile that can run Python3 and execute a custom 
script in a file named “myscript.py”.
```
FROM python:3.8

WORKDIR /data
COPY myscript.py .

CMD [ "python", "./myscript.py" ]
```

On the next page we are going to create an register predefined containers used 
for this workshop. For more information about these container, navigate to the 
Cloud9 environment and check the `code/containers` folder.