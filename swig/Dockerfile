# Build swig here
FROM debian:stretch AS builder

ARG SWIG_VERSION=4.0.1

RUN sed -Ee 's/^deb /deb-src /' /etc/apt/sources.list >> /etc/apt/sources.list.d/debsrc.list \
    && apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -q -y --no-install-recommends \
        build-essential \
        man \
        wget \
    && DEBIAN_FRONTEND=noninteractive apt-get build-dep -q -y --no-install-recommends \
        swig3.0 \
    && rm -rf /var/lib/apt/lists/*
RUN wget "https://sourceforge.net/projects/swig/files/swig/swig-${SWIG_VERSION}/swig-${SWIG_VERSION}.tar.gz" \
    && tar xzf "swig-${SWIG_VERSION}.tar.gz"
RUN cd "swig-${SWIG_VERSION}/" \
    && ./configure \
    && make \
    && make install

# Runtime image
FROM debian:stretch-slim

RUN adduser -q swig --disabled-password --gecos '' \
    && mkdir /src \
    && apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -q -y --no-install-recommends \
        libc6 \
        libgcc1 \
        libpcre3 \
        libstdc++6 \
        zlib1g \
        make \
        python \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/ /usr/local/

USER swig
WORKDIR /src