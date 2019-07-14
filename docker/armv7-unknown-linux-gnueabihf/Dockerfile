FROM ubuntu:18.04

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

RUN apt-get install -y --no-install-recommends \
    g++-arm-linux-gnueabihf \
    libc6-dev-armhf-cross

COPY openssl.sh /
RUN bash /openssl.sh linux-armv4 arm-linux-gnueabihf-

COPY qemu.sh /
RUN bash /qemu.sh arm linux softmmu

COPY dropbear.sh /
RUN bash /dropbear.sh

RUN apt-get install -y --no-install-recommends gnupg gnupg2

COPY linux-image.sh /
RUN bash /linux-image.sh armv7

COPY linux-runner /

ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_LINKER=arm-linux-gnueabihf-gcc \
    CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_RUNNER="/linux-runner armv7" \
    CC_armv7_unknown_linux_gnueabihf=arm-linux-gnueabihf-gcc \
    CXX_armv7_unknown_linux_gnueabihf=arm-linux-gnueabihf-g++ \
    OPENSSL_DIR=/openssl \
    OPENSSL_INCLUDE_DIR=/openssl/include \
    OPENSSL_LIB_DIR=/openssl/lib \
    QEMU_LD_PREFIX=/usr/arm-linux-gnueabihf \
    RUST_TEST_THREADS=1

# we need to add armhf architecture so that apt-get will allow us to install the
# packages for armhf later on
RUN dpkg --add-architecture armhf
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-updates main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://raspbian.raspberrypi.org/raspbian/ buster-updates main contrib non-free rpi" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://archive.raspberrypi.org/debian/ buster main ui untested" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://archive.raspberrypi.org/debian/ buster-updates main ui untested" >> /etc/apt/sources.list

# https://www.raspbian.org/RaspbianRepository
RUN apt-get install -y wget && wget https://archive.raspbian.org/raspbian.public.key -O - | apt-key add -

RUN apt-get update | true
# ll /usr/lib/arm-linux-gnueabihf/libSDL2-2.0.so.0
# ldconfig -p | grep SDL
#     libSDL2-2.0.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libSDL2-2.0.so.0
#     libSDL2-2.0.so (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libSDL2-2.0.so
#     libSDL_image-1.2.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libSDL_image-1.2.so.0
#     libSDL-1.2.so.0 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libSDL-1.2.so.0
#
# echo $LIBRARY_PATH 

# https://askubuntu.com/questions/1061979/cross-compile-for-armhf-and-install-a-static-library

# Remove all containers: docker rm $(docker ps -aq)
RUN apt-get install -y libsdl2-dev:armhf libsdl2-image-dev:armhf

