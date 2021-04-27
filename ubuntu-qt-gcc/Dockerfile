FROM ubuntu:20.04
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
    python3-pip
    
RUN pip3 install robotframework

# Install lcov for coverage
RUN apt-get install -y \
	lcov

# Install Qt
RUN apt-get install -y \
	qt5-default \
	libqt5charts5-dev

# Clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Workaround to fix "libQt5Core.so.5: cannot open shared object file: No such file or directory"
RUN strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5

WORKDIR /build

# Set up a volume for the generated reports
VOLUME /robot
