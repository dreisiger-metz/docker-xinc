FROM debian:bullseye
LABEL maintainer="hallo@petermetz.de"


RUN apt-get update && apt-get dist-upgrade -y -o Dpkg::Options::="--force-confold" && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        less vim screen sudo pulseaudio libegl1 xterm firefox-esr && \
    adduser --disabled-password --gecos "Default User" default && adduser default sudo && \
    cp /etc/sudoers /etc/sudoers.orig && sed 's/ ALL$/ NOPASSWD:ALL/' /etc/sudoers.orig > /etc/sudoers && \
    ln -sf /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get clean && rm -rf /var/lib/apt/lists* /tmp/* /var/tmp/*
    

USER default
CMD ["/usr/bin/xterm", "-fn", "10x20", "-geometry", "132x40"]
