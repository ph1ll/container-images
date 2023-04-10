# syntax=docker/dockerfile:1

FROM ghcr.io/ph1ll/container-images/alpine:3.17.3

ARG VERSION

RUN <<EOT
    : "${VERSION:?'Build argument needs to be set and non-empty.}"
EOT

RUN <<EOT
    wget \
        --quiet \
        https://github.com/nzbget/nzbget/releases/download/v${VERSION}/nzbget-${VERSION}-bin-linux.run \
        -O /tmp/nzbget.run
    chmod +x /tmp/nzbget.run
    /tmp/nzbget.run \
        --destdir /opt/nzbget
    rm /tmp/nzbget.run
EOT

EXPOSE 6789/tcp

VOLUME /config

COPY entrypoint /bin/entrypoint

HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:6789 || exit 1

ENTRYPOINT ["/bin/entrypoint"]

CMD ["/opt/nzbget/nzbget", "--server", "--configfile", "/config/nzbget.conf"] 