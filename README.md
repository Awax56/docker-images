# Docker Images

Purpose of this project is to share various commonly used docker files.

## How to build docker file

```console
cd <docker-folder>
sudo docker build -t <image_name> - < Dockerfile
```

Or with proxy:
```console
sudo docker build -t <image_name> --build-arg http_proxy=http://<ip>:<port> --build-arg https_proxy=http://<ip>:<port> - < Dockerfile
```

## How to use the docker image

For example to cross-compile some code using docker:

```console
sudo docker run -it --rm -v <path-on-host>:/build jlesauce/<image_name>:latest /bin/bash
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain-mingw-w64-x86-64.cmake .
make
```

## How to publish image to dockerhub

```console
docker login
docker build -t jlesauce/<image-name> .
docker push jlesauce/<image-name>
```

## Useful commands:

- List the containers:
    ```console
    sudo docker ps -a
    ```

- Clean unused containers:
    ```console
    sudo docker container prune
    ```

- Use already existing container:
    ```console
    sudo docker start <container_id>
    ```

- Clean docker completely on host machine:
    ```console
    sudo docker system prune
    ```
