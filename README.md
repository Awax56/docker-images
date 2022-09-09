# Docker Images

Purpose of this project is to share various commonly used docker files.

## How to build docker file

```console
cd <docker-folder>
sudo docker build - < Dockerfile
```

Or with proxy:

```console
sudo docker build --build-arg http_proxy=http://<ip>:<port> --build-arg https_proxy=http://<ip>:<port> - < Dockerfile
```

## How to use the docker image

For example to cross-compile some code using docker:
```console
sudo docker run -it --rm -v <path>:/build jlesauce/ubuntu-qt-mingw64:latest /bin/bash
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain-mingw-w64-x86-64.cmake .
make
```

## Useful commands:

List the containers:

```console
sudo docker ps -a
```

Clean unused containers:

```console
sudo docker container prune 
```

Use already existing container:

```console
sudo docker start <container_id>
```
