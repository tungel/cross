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

COPY qemu.sh /
RUN bash /qemu.sh arm

COPY musl.sh /
RUN bash /musl.sh \
    TARGET=arm-linux-musleabihf \
    "COMMON_CONFIG += --with-arch=armv7-a \
                      --with-fpu=neon \
                      --with-float=hard \
                      --with-mode=thumb"

COPY openssl.sh /
RUN bash /openssl.sh linux-armv4 arm-linux-musleabihf-

# Allows qemu run dynamic linked binaries
RUN ln -sf \
    /usr/local/arm-linux-musleabihf/lib/libc.so \
    /usr/local/arm-linux-musleabihf/lib/ld-musl-armhf.so.1
# ENV QEMU_LD_PREFIX=/usr/local/arm-linux-musleabihd
ENV QEMU_LD_PREFIX=/usr/local/arm-linux-musleabihf

ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_MUSLEABIHF_LINKER=arm-linux-musleabihf-gcc \
    CARGO_TARGET_ARMV7_UNKNOWN_LINUX_MUSLEABIHF_RUNNER=qemu-arm \
    CC_armv7_unknown_linux_musleabihf=arm-linux-musleabihf-gcc \
    CXX_armv7_unknown_linux_musleabihf=arm-linux-musleabihf-g++ \
    OPENSSL_DIR=/openssl \
    OPENSSL_INCLUDE_DIR=/openssl/include \
    OPENSSL_LIB_DIR=/openssl/lib \
    RUST_TEST_THREADS=1

RUN dpkg --add-architecture armhf
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic main multiverse restricted universe" >> /etc/apt/sources.list
RUN echo "deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports/ bionic-updates main multiverse restricted universe" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://raspbian.raspberrypi.org/raspbian/ buster-updates main contrib non-free rpi" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://archive.raspberrypi.org/debian/ buster main ui untested" >> /etc/apt/sources.list
# RUN echo "deb [arch=armhf] http://archive.raspberrypi.org/debian/ buster-updates main ui untested" >> /etc/apt/sources.list

# https://www.raspbian.org/RaspbianRepository
# RUN apt-get install -y wget && wget https://archive.raspbian.org/raspbian.public.key -O - | apt-key add -

RUN apt-get update | true
RUN apt-get install -y --no-install-recommends libsdl2-dev:armhf libsdl2-image-dev:armhf
RUN apt-get install -y --no-install-recommends curl vim

# RUN cp /usr/include/arm-linux-gnueabihf/sys/cdefs.h /usr/local/arm-linux-musleabihf/include/sys/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/wordsize.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/long-double.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/byteswap.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/types.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/typesizes.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp /usr/include/arm-linux-gnueabihf/bits/byteswap-16.h /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp -r -n /usr/include/arm-linux-gnueabihf/bits/* /usr/local/arm-linux-musleabihf/include/bits/
# RUN cp -r /usr/include/arm-linux-gnueabihf/gnu /usr/local/arm-linux-musleabihf/include/

# this doens't work, they are not valid musl libs
# RUN cp -r /usr/lib/arm-linux-gnueabihf/libSDL2* /usr/local/arm-linux-musleabihf/lib/
