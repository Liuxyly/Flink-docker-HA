# Create an image to build Hadoop nativelibs
#
# docker build -t sequenceiq/hadoop-nativelibs .

FROM maven:3-jdk-8-slim

ENV HADOOP_VERSION 2.8.5
ENV PROTOBUF_REVISION 3.6.1
ENV LD_LIBRARY_PATH /usr/local/lib
ENV LD_RUN_PATH /usr/local/lib

USER root

# install hadoop nativelins tools, install dev tools, devel tools
RUN set -x \
    apt-get update -y && \
    # yum groupinstall "Development Tools" -y && \
    apt-get install -y curl which tar sudo openssh-server openssh-clients rsync bunzip2 unzip \
        gcc gcc-c++ autoconf automake libtool zlib-devel cmake java-1.8.0-openjdk zlib-devel openssl-devel

# install protoc
RUN curl -sLO https://github.com/google/protobuf/releases/download/v${PROTOBUF_REVISION}/protoc-${PROTOBUF_REVISION}-linux-x86_64.zip \
  && unzip protoc-${PROTOBUF_REVISION}-linux-x86_64.zip -d ./usr/local \
  && chmod +x /usr/local/bin/protoc \
  && chmod -R 755 /usr/local/include/ \
  && rm protoc-${PROTOBUF_REVISION}-linux-x86_64.zip

# hadoop and build native libs
RUN set -x \
    && curl -s http://www.eu.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}-src.tar.gz | tar -xz -C /tmp/ && \
    cd /tmp/hadoop-$HADOOP_VERSION-src && mvn package -Pdist,native -DskipTests -Dtar

# clean image
RUN set -x \
    # yum groupremove "Development Tools" -y && \
    apt-get uninstall -y curl which tar sudo openssh-server openssh-clients rsync bunzip2 unzip \
        gcc gcc-c++ autoconf automake libtool zlib-devel cmake java-1.8.0-openjdk zlib-devel openssl-devel && \
    rm -rf /tmp/protobuf-all-$PROTOBUF_VERSION

# tar to stdout
CMD tar -cv -C /tmp/hadoop-$HADOOP_VERSION-src/hadoop-dist/target/hadoop-$HADOOP_VERSION/lib/native/ .

# docker run --rm  sequenceiq/hadoop-nativelibs > x.tar
# get bintray helper
#RUN curl -Lo /tmp/bintray-functions j.mp/bintray-functions && . /tmp/bintray-functions