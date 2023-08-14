FROM scratch as base

ARG VERSION

ADD rootfs-${VERSION}.tar /

COPY root/ /

RUN <<EOT
  adduser --gecos "Container User" --shell /bin/sh --disabled-password --uid 1000 container-user
EOT

USER container-user

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD [ "/bin/true" ]

ENTRYPOINT ["/etc/container-init"]

CMD ["tail", "-f", "/dev/null"]