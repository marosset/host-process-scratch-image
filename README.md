# Host-Process container scratch image

## Overview

This project produces a minimal "scratch" image that can be used with [HostProcess containers](https://kubernetes.io/docs/tasks/configure-pod-container/create-hostprocess-pod/).

### Benefits

Using this image as a bash for HostProcess containers has a few advantages over using other base images for Windows containers including:

- Size - This image is a few KB. Even the smallest official base image (NanoServer) is still a few hundred MB is size.
- OS compatibility - HostProcess containers do not inherit the same [compatibility requirements](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility) as Windows server containers and because of this it does not make sense to include all of the runtime / system binaries that make up the different base layers. Using this image allows for a single container image to be used on any Windows Server version which can greatly simplify container build processes.

## Usage

Build your container from `ghcr.io/marosset/host-process-scratch-image:latest`.

### Dockerfile example

```yaml
FROM ghcr.io/marosset/host-process-scratch-image:latest

ADD hello-world.ps1 .

ENV PATH="C:\Windows\system32;C:\Windows;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;"
ENTRYPOINT ["powershell.exe", "./hello-world.ps1"]
```

### Build with BuildKit

Containers based on this image cannot currently be built with Docker Desktop.
Instead use BuildKit or other tools.

Example:

#### Create a builder

One time step

```cmd
docker buildx create --name img-builder --use --platform windows/and64
```

#### Build your image

Use the following command to build and push to a container reposity

```cmd
 docker buildx build --platform windows/amd64 --output=type=registry -f {Dockerfile} -t {ImageTag} .
```
