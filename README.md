# Singularity in Docker

[![Docker images available at kaczmarj/singularity](https://img.shields.io/badge/DockerHub-kaczmarj/singularity-blue)](https://hub.docker.com/r/kaczmarj/singularity)
[![Docker pulls](https://img.shields.io/docker/pulls/kaczmarj/singularity)](https://hub.docker.com/r/kaczmarj/singularity)

You've heard of [Docker in Docker](https://github.com/jpetazzo/dind), but what about Singularity in Docker?

The Dockerfile in this repository builds Singularity 3.x. The resulting Docker image can be used on any system with Docker to build Singularity images. This project is targeted towards high-performance computing users who have Singularity installed on their clusters but do not have Singularity on their local computers to build images.

## Convert local Docker image to Singularity format

In the following example, we convert an existing Docker image to Singularity format.

```bash
$ docker pull alpine:3.11
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/work \
    kaczmarj/singularity:3.8.0 build alpine_3.11.sif docker-daemon://alpine:3.11
```

This output `.sif` file will be owned by root, so you can change ownership:

```bash
sudo chown USER:GROUP alpine_3.11.sif
```

## Build Singularity image in Docker

With the following command, we build a small Singularity image defined in [`Singularity_test`](Singularity_test). This Singularity image will be saved in the current directory `myimage.sif`.

```bash
$ docker run --rm --privileged -v $(pwd):/work kaczmarj/singularity:3.8.0 \
  build myimage.sif Singularity_test
```

## Run Singularity image in Docker

One can run a Singularity image within this Docker image. This is not recommended, but it is possible.

```bash
$ docker run --rm --privileged kaczmarj/singularity:3.8.0 \
  run shub://GodloveD/lolcow
```

Here is the output:

```
INFO:    Downloading shub image
87.6MiB / 87.6MiB [======================================================================================================================================================] 100 % 1.4 MiB/s 0s
 _________________________________________
/ He that is giddy thinks the world turns \
| round.                                  |
|                                         |
| -- William Shakespeare, "The Taming of  |
\ the Shrew"                              /
 -----------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

## Build image

Version 3.8.0

```bash
$ docker build --build-arg SINGULARITY_COMMITISH=v3.8.0 -t singularity:3.8.0 - < Dockerfile
```

Bleeding-edge (master branch):

```bash
$ docker build --build-arg SINGULARITY_COMMITISH=master -t singularity:latest - < Dockerfile
```
