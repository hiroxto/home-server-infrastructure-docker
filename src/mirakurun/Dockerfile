# syntax=docker/dockerfile:1

FROM chinachu/mirakurun:3.8.0

ARG DEPENDENCIES="git build-essential libtool autoconf automake cmake pkg-config libpcsclite-dev"
RUN <<EOF
    set -eux
    sed -i 's/deb.debian.org/archive.debian.org/g' /etc/apt/sources.list
    sed -i 's/security.debian.org/archive.debian.org/g' /etc/apt/sources.list
    sed -i '/buster-updates/d' /etc/apt/sources.list
    apt-get update
    apt-get install -y ${DEPENDENCIES}
    apt-get install -y libpcsclite1 pcscd ccid
    # Install libaribb25
    git clone https://github.com/tsukumijima/libaribb25 /tmp/libaribb25
    cd /tmp/libaribb25
    cmake .
    make
    make install
    # Install recpt1
    git clone https://github.com/stz2012/recpt1 /tmp/recpt1
    cd /tmp/recpt1/recpt1
    ./autogen.sh
    ./configure
    make
    make install
    # Cleanup
    ldconfig
    rm -rf /tmp/libaribb25 /tmp/recpt1
    apt-get -y remove ${DEPENDENCIES}
    apt-get autoremove -y
    apt-get clean
    rm -rf /var/lib/apt/lists/*
EOF
