FROM ubuntu:bionic
RUN export DEBIAN_FRONTEND="noninteractive" \
    && apt-get update \
    && apt-get install -y --install-recommends \
        apt-transport-https \
        ca-certificates \
        pulseaudio-utils \
        software-properties-common \
        unzip \
        wget \
        winbind
RUN wget https://dl.winehq.org/wine-builds/winehq.key \
    && apt-key add winehq.key \
    && apt-add-repository "deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main" \
    && dpkg --add-architecture i386 \
    && apt-get update \
    && apt-get install -y aptitude libnvidia-gl-418:i386 \
    && aptitude install -y wine-stable
WORKDIR /root/.wine/drive_c/everquest
CMD ["/bin/bash"]
