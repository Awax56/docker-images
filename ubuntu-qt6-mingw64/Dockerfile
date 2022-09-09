FROM ubuntu:22.04
MAINTAINER Julien Le Sauce

ENV DEBIAN_FRONTEND=noninteractive

#-----------------------------
#    Install dependencies
#-----------------------------

RUN apt-get update

# Install common dependencies
RUN apt-get install -y --no-install-recommends \
    apt-utils \
    software-properties-common \
    ca-certificates \
    curl \
    python3 \
    git \
    bash

# Install compilation tools
RUN apt-get install -y \
    build-essential \
    mingw-w64 \
    make  \
    cmake \
    ninja-build

# Install Qt6 for host
RUN add-apt-repository universe
RUN add-apt-repository multiverse
RUN apt-get update
RUN apt-get install -y \
    qt6-base-dev \
    qt6-tools-dev-tools

# Clean apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

#-----------------------------
#    Compile Qt from sources
#-----------------------------

# Qt version
ARG QT_VERSION_MAJOR=6
ARG QT_VERSION_MINOR=2
ARG QT_VERSION_PATCH=4
ARG QT_VERSION=${QT_VERSION_MAJOR}.${QT_VERSION_MINOR}
ARG QT_VERSION_FULL=${QT_VERSION}.${QT_VERSION_PATCH}
ARG QT_DOWNLOAD_BRANCH=official_releases

# Qt build/install dirs
ENV QT_SDK_DIR=/opt/qt-sdk-${QT_VERSION_FULL}
ENV QT_BUILD_ROOT=/tmp/qt_build
ENV QT_BUILD_DIR=${QT_BUILD_ROOT}/qt-everywhere-src-${QT_VERSION}.${QT_VERSION_PATCH}/build
ENV QT_INSTALL_DIR=/opt/Qt6/mingw_64

# Download Qt sources
WORKDIR ${QT_BUILD_ROOT}
ARG QT_SOURCES_URL=https://download.qt.io/${QT_DOWNLOAD_BRANCH}/qt/${QT_VERSION}/${QT_VERSION_FULL}/single
ARG QT_SOURCES_TARBALL=qt-everywhere-src-${QT_VERSION_FULL}.tar.xz
RUN curl -sSL ${QT_SOURCES_URL}/${QT_SOURCES_TARBALL} | tar xJ

WORKDIR qt-everywhere-src-${QT_VERSION_FULL}

# Download toolchain for cmake cross-compilation for mingw-w64
RUN curl -sSL https://raw.githubusercontent.com/jlesauce/docker-images/master/toolchain-mingw-w64-x86-64.cmake -o /tmp/toolchain-mingw-w64-x86-64.cmake

# Configure, compile and install
RUN ./configure \
    -prefix ${QT_SDK_DIR}/mingw_64 \
    -qt-host-path /usr/lib/x86_64-linux-gnu/cmake \
    -extprefix ${QT_INSTALL_DIR} \
    -release \
    -opengl es2 \
    -nomake examples \
    -nomake tests \
    -skip qt3d \
    -skip qt5compat \
    -skip qtactiveqt \
    -skip qtcharts \
    -skip qtcoap \
    -skip qtconnectivity \
    -skip qtdatavis3d \
    -skip qtdeclarative \
    -skip qtdoc \
    -skip qtimageformats \
    -skip qtlanguageserver \
    -skip qtlottie \
    -skip qtmqtt \
    -skip qtmultimedia \
    -skip qtnetworkauth \
    -skip qtopcua \
    -skip qtpositioning \
    -skip qtquick3d \
    -skip qtquicktimeline \
    -skip qtremoteobjects \
    -skip qtscxml \
    -skip qtsensors \
    -skip qtserialbus \
    -skip qtserialport \
    -skip qtshadertools \
    -skip qtsvg \
    -skip qttools \
    -skip qttranslations \
    -skip qtvirtualkeyboard \
    -skip qtwayland \
    -skip qtwebchannel \
    -skip qtwebengine \
    -skip qtwebsockets \
    -skip qtwebview \
    -- \
    -DCMAKE_TOOLCHAIN_FILE=/tmp/toolchain-mingw-w64-x86-64.cmake

RUN cmake --build . --parallel
RUN cmake --install .

#-----------------------------
#    Configure environment
#-----------------------------

ENV PATH="${PATH}:${QT_SDK_DIR}/bin"
ENV CMAKE_PREFIX_PATH="${QT_SDK_DIR}/lib/cmake/Qt6"

WORKDIR /build
