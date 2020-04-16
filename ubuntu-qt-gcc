FROM ubuntu:18.04
MAINTAINER Julien Le Sauce version: 0.1

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get clean

# Install common dependencies
RUN apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y build-essential git

# Install C++ compiler
RUN apt-get install -y g++ cmake

# Install Qt
RUN apt-get install -y qt5-default

# Clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /build

ENTRYPOINT ["bash"]
