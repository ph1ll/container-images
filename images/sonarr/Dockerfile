FROM ghcr.io/ph1ll/container-images/alpine:3.18.3

USER root

ARG VERSION

# COPY root/ /

RUN <<EOT
  echo "https://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories
  apk add --no-cache mono mediainfo
  wget \
      --quiet \
      https://download.sonarr.tv/v3/main/${VERSION}/Sonarr.main.${VERSION}.linux.tar.gz \
      -O /tmp/Sonarr.main.${VERSION}.linux.tar.gz
  tar -xf /tmp/Sonarr.main.${VERSION}.linux.tar.gz -C /opt
  rm -f /tmp/Sonarr.main.${VERSION}.linux.tar.gz
  mv /opt/Sonarr /opt/sonarr
  cert-sync /etc/ssl/certs/ca-certificates.crt
  mkdir /config
  chown -R container-user:container-user /config
  mkdir /downloads
  chown -R container-user:container-user /downloads
  mkdir /shows
  chown -R container-user:container-user /shows
EOT

EXPOSE 8989/tcp

VOLUME /config
VOLUME /downloads
VOLUME /shows

USER container-user

HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8989 || exit 1

CMD ["mono", "--debug", "/opt/sonarr/Sonarr.exe", "-nobrowser", "-data=/config"]