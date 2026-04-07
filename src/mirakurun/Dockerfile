# syntax=docker/dockerfile:1

FROM chinachu/mirakurun:3.8.0

ARG DEPENDENCIES="build-essential libtool autoconf git automake cmake"
RUN <<EOF
    set -eux
    apt-get update && apt-get install -y ${DEPENDENCIES}
    # Install recpt1
    git clone https://github.com/stz2012/recpt1 /tmp/recpt1
    cd /tmp/recpt1/recpt1
    ./autogen.sh
    ./configure
    make
    make install
    rm -rf /tmp/recpt1
    # Cleanup
    apt-get -y remove ${DEPENDENCIES}
    apt-get autoremove -y
    apt-get clean
    rm -rf /var/lib/apt/lists/*
EOF
