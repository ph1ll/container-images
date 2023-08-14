FROM ghcr.io/ph1ll/container-images/alpine:3.18.3

USER root

ARG VERSION

# COPY root/ /

RUN <<EOT
  apk add --no-cache icu-libs libstdc++ sqlite-libs
  wget \
      --quiet \
      https://github.com/Radarr/Radarr/releases/download/v${VERSION}/Radarr.master.${VERSION}.linux-musl-core-x64.tar.gz \
      -O /tmp/Radarr.master.${VERSION}.linux-musl-core-x64.tar.gz
  tar -xf /tmp/Radarr.master.${VERSION}.linux-musl-core-x64.tar.gz -C /opt
  rm -f /tmp/Radarr.master.${VERSION}.linux-musl-core-x64.tar.gz
  mv /opt/Radarr /opt/radarr
  # cert-sync /etc/ssl/certs/ca-certificates.crt
  mkdir /config
  chown -R container-user:container-user /config
  mkdir /downloads
  chown -R container-user:container-user /downloads
  mkdir /movies
  chown -R container-user:container-user /movies
EOT

EXPOSE 7878/tcp

VOLUME /config
VOLUME /downloads
VOLUME /movies

USER container-user

HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:7878 || exit 1

CMD ["/opt/radarr/Radarr", "-nobrowser", "-data=/config"]