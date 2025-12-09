FROM ghcr.io/home-assistant/amd64-base-debian:bookworm

WORKDIR /opt/z-way-server

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -qqy --no-install-recommends \
    ca-certificates curl wget procps iproute2 openssh-client openssh-server sudo jq

RUN wget https://tridentiot.github.io/apt-repository/README.md -qO -  | grep -E '^    ' | sudo bash
RUN rm -f /opt/z-way-server/automation/storage/*

RUN echo "ha" > /etc/z-way/box_type

RUN useradd -rm -d /home/support -s /bin/bash -g root -G sudo -u 1000 support
RUN echo 'support:tridentiot' | chpasswd

# Manually install zbw
RUN mkdir -p /etc/zbw/flags && touch /etc/zbw/flags/no_connection && wget https://repo.z-wave.me/z-way/debian/pool/main/z/zbw/zbw_2.6_all.deb && sudo dpkg -i zbw_2.6_all.deb && rm zbw_2.6_all.deb && rm /etc/zbw/flags/no_connection

COPY rootfs/ /

EXPOSE 8083

ENTRYPOINT ["/init"]
