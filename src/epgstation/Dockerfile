# syntax=docker/dockerfile:1

FROM l3tnun/epgstation:v2.10.0-debian

ENV DEBIAN_FRONTEND=noninteractive

# renovate: datasource=github-tags depName=FFmpeg/FFmpeg
ARG FFMPEG_VERSION="n7.0"
ARG DEPENDENCIES="make gcc git g++ automake curl wget autoconf build-essential libass-dev libfreetype6-dev libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev pkg-config texinfo zlib1g-dev"

RUN <<EOF
    set -eux

    apt-get update
    apt-get -y install ${DEPENDENCIES}
    apt-get -y install yasm libx264-dev libmp3lame-dev libopus-dev libvpx-dev
    apt-get -y install libx265-dev libnuma-dev
    apt-get -y install libasound2 libass9 libvdpau1 libva-x11-2 libva-drm2 libxcb-shm0 libxcb-xfixes0 libxcb-shape0 libvorbisenc2 libtheora0 libaribb24-dev
    #ffmpeg build
    mkdir /tmp/ffmpeg_source
    git clone --branch=${FFMPEG_VERSION} --depth=1 --recursive https://github.com/FFmpeg/FFmpeg /tmp/ffmpeg_source
    cd /tmp/ffmpeg_source
    ./configure \
        --prefix=/usr/local \
        --disable-shared \
        --pkg-config-flags=--static \
        --enable-gpl \
        --enable-libass \
        --enable-libfreetype \
        --enable-libmp3lame \
        --enable-libopus \
        --enable-libtheora \
        --enable-libvorbis \
        --enable-libvpx \
        --enable-libx264 \
        --enable-libx265 \
        --enable-version3 \
        --enable-libaribb24 \
        --enable-nonfree \
        --disable-debug \
        --disable-doc
    make -j$(nproc)
    make install
    # timezone
    apt-get install -y tzdata
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
    # Cleanup
    apt-get -y remove ${DEPENDENCIES}
    apt-get autoremove -y
    apt-get clean
    rm -rf /var/lib/apt/lists/*
    rm -rf /tmp/*
EOF
