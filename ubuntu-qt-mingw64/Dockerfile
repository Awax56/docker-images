FROM ubuntu:20.04
MAINTAINER Julien Le Sauce

ENV DEBIAN_FRONTEND=noninteractive

#-----------------------------
#    Install dependencies
#-----------------------------

RUN apt-get update && apt-get clean

# Install common dependencies
RUN apt-get install -y --no-install-recommends \
    apt-utils \
    curl \
    python \
    git \
    bash

# Install C/C++ compiler
RUN apt-get install -y \
    build-essential \
    mingw-w64 \
    make  \
    cmake

# Clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

#-----------------------------
#    Compile Qt from sources
#-----------------------------

# QT Version
ARG QT_VERSION_MAJOR=5
ARG QT_VERSION_MINOR=12
ARG QT_VERSION_PATCH=8
ARG QT_VERSION=${QT_VERSION_MAJOR}.${QT_VERSION_MINOR}
ARG QT_VERSION_FULL=${QT_VERSION}.${QT_VERSION_PATCH}
ARG QT_DOWNLOAD_BRANCH=official_releases

ENV QT_SDK_DIR=/opt/qt-sdk-${QT_VERSION_FULL}
ENV QT_BUILD_ROOT=/tmp/qt_build
ENV QT_BUILD_DIR=${QT_BUILD_ROOT}/qt-everywhere-src-${QT_VERSION}.${QT_VERSION_PATCH}/build

# Download sources
WORKDIR ${QT_BUILD_ROOT}
ARG QT_SOURCES_URL=https://download.qt.io/${QT_DOWNLOAD_BRANCH}/qt/${QT_VERSION}/${QT_VERSION_FULL}/single
ARG QT_SOURCES_TARBALL=qt-everywhere-src-${QT_VERSION_FULL}.tar.xz
RUN curl -sSL ${QT_SOURCES_URL}/${QT_SOURCES_TARBALL} | tar xJ

WORKDIR qt-everywhere-src-${QT_VERSION_FULL}

# Configure, make, install
RUN ./configure \
    -xplatform win32-g++ \
    -device-option CROSS_COMPILE=/usr/bin/x86_64-w64-mingw32- \
    -prefix ${QT_SDK_DIR}/mingw_64 \
    -opensource \
    -no-compile-examples \
    -opengl desktop \
    -skip qt3d \
    -skip qtactiveqt \
    -skip qtandroidextras \
    -skip qtcanvas3d \
    -skip qtconnectivity \
    -skip qtdatavis3d \
    -skip qtdeclarative \
    -skip qtdoc \
    -skip qtgamepad \
    -skip qtgraphicaleffects \
    -skip qtimageformats \
    -skip qtlocation \
    -skip qtmacextras \
    -skip qtmultimedia \
    -skip qtnetworkauth \
    -skip qtpurchasing \
    -skip qtquickcontrols \
    -skip qtquickcontrols2 \
    -skip qtremoteobjects \
    -skip qtscript \
    -skip qtscxml \
    -skip qtsensors \
    -skip qtserialbus \
    -skip qtserialport \
    -skip qtspeech \
    -skip qttranslations \
    -skip qtvirtualkeyboard \
    -skip qtwayland \
    -skip qtwebchannel \
    -skip qtwebengine \
    -skip qtwebsockets \
    -skip qtwebview \
    -skip qtwinextras \
    -skip qtx11extras \
    -skip qtxmlpatterns
RUN make
RUN make install

ENV PATH="${PATH}:${QT_SDK_DIR}/bin"
ENV CMAKE_PREFIX_PATH="${QT_SDK_DIR}/lib/cmake/Qt5"

WORKDIR /build
