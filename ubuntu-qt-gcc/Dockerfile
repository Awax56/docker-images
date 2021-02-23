FROM ubuntu:18.04
MAINTAINER Julien Le Sauce

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get clean

# Install common dependencies
RUN apt-get install -y \
	build-essential \
	git \
	python3

# Install C/C++ compiler
RUN apt-get install -y \
    g++   \
    gcc   \
    make  \
    cmake
    
# Install Robot framework
RUN apt-get install -y \
    python3 \
    python-pip
    
RUN pip install robotframework

# Install Qt
RUN apt-get install -y \
	qt5-default

# Clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /build

# Set up a volume for the generated reports
VOLUME /robot
