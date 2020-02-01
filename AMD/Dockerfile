FROM ubuntu:bionic
RUN apt-get update \
    && apt-get install -y --install-recommends \
        apt-transport-https \
        ca-certificates \
        software-properties-common \
        wget 
RUN export DEBIAN_FRONTEND="noninteractive" \
    && wget https://dl.winehq.org/wine-builds/winehq.key \
    && apt-key add winehq.key \
    && apt-add-repository "deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main" \
    && dpkg --add-architecture i386 \
    && apt-get update \
    && apt-get install -y --install-recommends wine32 winetricks winbind
RUN mkdir -p /usr/share/wine/gecko \
    && wget https://dl.winehq.org/wine/wine-gecko/2.47/wine_gecko-2.47-x86.msi \
        -O /usr/share/wine/gecko/wine_gecko-2.47-x86.msi \
    && wget https://dl.winehq.org/wine/wine-gecko/2.47/wine_gecko-2.47-x86_64.msi \
        -O /usr/share/wine/gecko/wine_gecko-2.47-x86_64.msi
WORKDIR /root/.wine/drive_c/everquest
CMD ["/bin/bash"]
