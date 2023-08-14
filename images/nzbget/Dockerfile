FROM ghcr.io/ph1ll/container-images/alpine:3.18.3

USER root

ARG VERSION

COPY root/ /

RUN <<EOT
    wget \
        --quiet \
        https://github.com/nzbget/nzbget/releases/download/v${VERSION}/nzbget-${VERSION}-bin-linux.run \
        -O /tmp/nzbget.run
    chmod +x /tmp/nzbget.run
    /tmp/nzbget.run \
        --destdir /opt/nzbget
    rm /tmp/nzbget.run
    mkdir /config
    chown -R container-user:container-user /config
    mkdir /downloads
    chown -R container-user:container-user /downloads
EOT

EXPOSE 6789/tcp

VOLUME /config
VOLUME /downloads

USER container-user

HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:6789 || exit 1

CMD ["/opt/nzbget/nzbget", "--server", "--configfile", "/config/nzbget.conf"]