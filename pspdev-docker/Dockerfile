FROM ubuntu:17.10

ENV PSPDEV /usr/local/pspdev
ENV PATH ${PATH}:${PSPDEV}/bin

RUN \
  apt-get -y update && \
  apt-get -y install autoconf bison build-essential cmake doxygen flex git libncurses5-dev libreadline-dev libsdl1.2-dev libtool-bin libusb-dev subversion tcl texinfo unzip wget zlib1g-dev && \
  apt-get -y clean autoclean autoremove && \
  rm -rf /var/lib/{apt,dpkg,cache,log}/

RUN \
  git clone https://github.com/suicvne/psptoolchain && \
  cd psptoolchain && \
    ./toolchain.sh && \
  cd .. && \
  rm -Rf psptoolchain

RUN echo 'Hello World!!!'

WORKDIR /build