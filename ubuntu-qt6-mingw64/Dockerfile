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

# Qt build dir
ENV QT_SDK_DIR=/opt/qt-sdk-${QT_VERSION_FULL}
ENV QT_BUILD_ROOT=/tmp/qt_build
ENV QT_BUILD_DIR=${QT_BUILD_ROOT}/qt-everywhere-src-${QT_VERSION}.${QT_VERSION_PATCH}/build

# Download Qt sources
WORKDIR ${QT_BUILD_ROOT}
ARG QT_SOURCES_URL=https://download.qt.io/${QT_DOWNLOAD_BRANCH}/qt/${QT_VERSION}/${QT_VERSION_FULL}/single
ARG QT_SOURCES_TARBALL=qt-everywhere-src-${QT_VERSION_FULL}.tar.xz
RUN curl -sSL ${QT_SOURCES_URL}/${QT_SOURCES_TARBALL} | tar xJ

WORKDIR qt-everywhere-src-${QT_VERSION_FULL}

# Download toolchain for cmake cross-compilation
RUN curl -sSL https://raw.githubusercontent.com/jlesauce/docker-images/master/toolchain-mingw-w64-x86-64.cmake -o /tmp/toolchain-mingw-w64-x86-64.cmake


RUN apt-get update
RUN apt-get install -y libqt6shadertools6-dev
RUN apt-get install -y libboost-all-dev libudev-dev libinput-dev libts-dev libmtdev-dev libjpeg-dev libfontconfig1-dev libssl-dev libdbus-1-dev libglib2.0-dev libxkbcommon-dev libegl1-mesa-dev libgbm-dev libgles2-mesa-dev mesa-common-dev libasound2-dev libpulse-dev gstreamer1.0-omx libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev  gstreamer1.0-alsa libvpx-dev libsrtp2-dev libsnappy-dev libnss3-dev "^libxcb.*" flex bison libxslt-dev ruby gperf libbz2-dev libcups2-dev libatkmm-1.6-dev libxi6 libxcomposite1 libfreetype6-dev libicu-dev libsqlite3-dev libxslt1-dev
RUN apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libx11-dev freetds-dev libsqlite3-dev libpq-dev libiodbc2-dev firebird-dev libgst-dev libxext-dev libxcb1 libxcb1-dev libx11-xcb1 libx11-xcb-dev libxcb-keysyms1 libxcb-keysyms1-dev libxcb-image0 libxcb-image0-dev libxcb-shm0 libxcb-shm0-dev libxcb-icccm4 libxcb-icccm4-dev libxcb-sync1 libxcb-sync-dev libxcb-render-util0 libxcb-render-util0-dev libxcb-xfixes0-dev libxrender-dev libxcb-shape0-dev libxcb-randr0-dev libxcb-glx0-dev libxi-dev libdrm-dev libxcb-xinerama0 libxcb-xinerama0-dev libatspi2.0-dev libxcursor-dev libxcomposite-dev libxdamage-dev libxss-dev libxtst-dev libpci-dev libcap-dev libxrandr-dev libdirectfb-dev libaudio-dev libxkbcommon-x11-dev
RUN apt-get install -y qml-qt6 qml6-module-qtqml qml6-module-qtcore

# ????
RUN mkdir $HOME/qt-mingw_64

# Configure, compile and install
# RUN ./configure \
#     -prefix ${QT_SDK_DIR}/mingw_64 \
#     -platform linux-g++ \
#     -xplatform win32-g++ \
#     -qt-host-path $HOME/qt-host \
#     -device-option CROSS_COMPILE=/usr/bin/x86_64-w64-mingw32- \
#     -opensource \
#     -opengl desktop \
#     -nomake tests \
#     -nomake examples \
#     -- \
#     -DQT_BUILD_TOOLS_WHEN_CROSSCOMPILING=ON \
#     -DCMAKE_TOOLCHAIN_FILE=/tmp/toolchain-mingw-w64-x86-64.cmake

RUN ./configure \
    -prefix ${QT_SDK_DIR}/mingw_64 \
    -qt-host-path /usr/lib/x86_64-linux-gnu/cmake \
    -extprefix $HOME/qt6-mingw_64 \
    -release \
    -opengl es2 \
    -nomake examples \
    -nomake tests \
    -skip qt3d \
    -skip qtactiveqt \
    -skip qtandroidextras \
    -skip qtcanvas3d \
    -skip qtconnectivity \
    -skip qttools \
    -skip qtdatavis3d \
    -skip qtquick3d \
    -skip qtquicktimeline \
    -skip qtopcua \
    -skip qtmqtt \
    -skip qtlottie \
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
    -skip qtxmlpatterns \
    -- -DCMAKE_TOOLCHAIN_FILE=/tmp/toolchain-mingw-w64-x86-64.cmake

RUN cmake --build . --parallel
RUN cmake --install .

#-----------------------------
#    Configure environment
#-----------------------------

ENV PATH="${PATH}:${QT_SDK_DIR}/bin"
ENV CMAKE_PREFIX_PATH="${QT_SDK_DIR}/lib/cmake/Qt6"

WORKDIR /build
