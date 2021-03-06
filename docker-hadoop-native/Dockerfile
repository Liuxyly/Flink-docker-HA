# Create an image to build Hadoop native builder
#
# docker build -t liuxyly/hadoop-native-builder .
FROM maven:3-jdk-8-alpine

ENV HADOOP_VERSION 2.8.5
ENV PROTOBUF_REVISION 2.5.0
ENV LD_LIBRARY_PATH /usr/local/lib
ENV LD_RUN_PATH /usr/local/lib

USER root

# install hadoop nativelins tools
RUN set -x \
    && apk update && \
    apk add --no-cache flex bison gperf alpine-sdk \
        subversion curl zlib-dev build-base autoconf automake libtool ccache cmake \
        libxml2-dev openssl-dev

# RUN set -x \
#   && sed -i '/\/usr\/lib\/jvm\/java-8-openjdk-{libarch}\/include/a \/usr\/lib\/jvm\/java-8-openjdk-amd64\/include' $(find / -name FindJNI.cmake) \
#   && sed -i '/\/usr\/lib\/jvm\/java-8-openjdk-{libarch}\/jre\/lib\/{libarch}/a \/usr\/lib\/jvm\/java-8-openjdk-amd64\/jre\/lib\/{libarch}' $(find / -name FindJNI.cmake)

# install protoc
# RUN curl -sLO https://github.com/google/protobuf/releases/download/v${PROTOBUF_REVISION}/protoc-${PROTOBUF_REVISION}-linux-x86_64.zip \
#   && unzip protoc-${PROTOBUF_REVISION}-linux-x86_64.zip -d ./usr/local \
#   && chmod +x /usr/local/bin/protoc \
#   && chmod -R 755 /usr/local/include/ \
#   && rm protoc-${PROTOBUF_REVISION}-linux-x86_64.zip

RUN set -x \
    && cd /tmp/ && \
    curl -sLO https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOBUF_REVISION}/protobuf-${PROTOBUF_REVISION}.tar.gz && \
    tar -xf protobuf-${PROTOBUF_REVISION}.tar.gz && \
    cd protobuf-${PROTOBUF_REVISION} && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install

# hadoop and build native libs
RUN set -x \
    && curl -s http://www.eu.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}-src.tar.gz | tar -xz -C /tmp/ && \
    cd /tmp/hadoop-$HADOOP_VERSION-src && mvn package -Pdist,native -DskipTests -Dtar && \
    mkdir /output && \
    cp -r /tmp/hadoop-$HADOOP_VERSION-src/hadoop-dist/target/hadoop-$HADOOP_VERSION/lib/native/* /output/ && \
    rm -rf /tmp/hadoop-$HADOOP_VERSION-src

# clean image
RUN set -x \
    && apk del flex bison gperf alpine-sdk \
        subversion curl zlib-dev build-base autoconf automake libtool ccache cmake \
        libxml2-dev openssl-dev \
    && rm -rf /root/*

# tar to stdout
CMD tar -cv -C /output .

# docker run --rm  sequenceiq/hadoop-nativelibs > x.tar
# get bintray helper
#RUN curl -Lo /tmp/bintray-functions j.mp/bintray-functions && . /tmp/bintray-functions