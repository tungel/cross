FROM ubuntu:14.04
# FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    cmake \
    gcc \
    libc6-dev \
    make \
    pkg-config \
    git \
    automake \
    libtool \
    m4 \
    autoconf \
    make \
    file \
    binutils

COPY xargo.sh /
RUN bash /xargo.sh

COPY cmake.sh /
RUN apt-get purge --auto-remove -y cmake && \
    bash /cmake.sh 3.5.1

RUN mkdir /usr/arm-linux-gnueabihf && \
    apt-get install -y --no-install-recommends curl && \
    cd /usr/arm-linux-gnueabihf && \
    curl -L https://toolchains.bootlin.com/downloads/releases/toolchains/armv6-eabihf/tarballs/armv6-eabihf--glibc--stable-2018.11-1.tar.bz2 | \
    tar --strip-components 1 -xj && \
    apt-get purge --auto-remove -y curl

COPY openssl.sh qemu.sh /

RUN bash /qemu.sh arm
ENV PATH /usr/arm-linux-gnueabihf/bin:$PATH

RUN apt-get install -y --no-install-recommends \
    libc6-dev-armhf-cross && \
    bash /openssl.sh linux-armv4 arm-linux-

ENV CARGO_TARGET_ARM_UNKNOWN_LINUX_GNUEABIHF_LINKER=arm-linux-gcc \
    CARGO_TARGET_ARM_UNKNOWN_LINUX_GNUEABIHF_RUNNER=qemu-arm \
    CC_arm_unknown_linux_gnueabihf=arm-linux-gcc \
    CXX_arm_unknown_linux_gnueabihf=arm-linux-g++ \
    OPENSSL_DIR=/openssl \
    OPENSSL_INCLUDE_DIR=/openssl/include \
    OPENSSL_LIB_DIR=/openssl/lib \
    QEMU_LD_PREFIX=/usr/arm-linux-gnueabihf \
    LD_LIBRARY_PATH=/usr/lib/arm-linux-gnueabihf:/usr/arm-linux-gnueabihf/lib:/usr/arm-linux-gnueabihf/arm-buildroot-linux-gnueabihf/lib \
    RUST_TEST_THREADS=1

# RUN apt-get install -y --no-install-recommends curl

# we need to add armhf architecture so that apt-get will allow us to install the
# packages for armhf later on


RUN apt update
# RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-updates main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-security main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-proposed main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-backports main multiverse restricted universe" >> /etc/apt/sources.list


RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ trusty main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ trusty-updates main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ trusty-security main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ trusty-proposed main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ trusty-backports main multiverse restricted universe" >> /etc/apt/sources.list

RUN apt update
RUN dpkg --add-architecture armhf
RUN apt install -y libsdl2-2.0-0:armhf libgles2-mesa-dev:armhf libglu1-mesa:armhf libsdl2-dev:armhf libsdl2-image-dev:armhf

RUN apt-get install -y --no-install-recommends curl

RUN qemu-arm-static /usr/arm-linux-gnueabihf/arm-buildroot-linux-gnueabihf/sysroot/sbin/ldconfig

# to disable docker build cache, put this line above the step that you want to
# disable cache
# ARG CACHEBUST=1

