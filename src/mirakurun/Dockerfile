# syntax=docker/dockerfile:1

FROM chinachu/mirakurun:4.1.3

ARG DEPENDENCIES="git build-essential libtool autoconf automake cmake"
RUN <<EOF
    set -eux
    apt-get update
    apt-get install -y ${DEPENDENCIES} pkg-config libpcsclite-dev
    # Install recpt1
    git clone https://github.com/stz2012/recpt1 /tmp/recpt1
    cd /tmp/recpt1/recpt1
    ./autogen.sh
    ./configure
    make
    make install
    # Cleanup
    ldconfig
    rm -rf /tmp/recpt1
    apt-get -y remove ${DEPENDENCIES}
    apt-get autoremove -y
    apt-get clean
    rm -rf /var/lib/apt/lists/*
EOF
