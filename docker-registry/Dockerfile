# Build a minimal distribution container

FROM alpine:3.8

RUN set -ex \
    && apk add --no-cache ca-certificates apache2-utils

COPY ./registry /bin/registry
COPY ./config-example.yml /etc/docker/registry/config.yml

RUN mkdir -p /var/lib/registry
EXPOSE 5000

COPY docker-entrypoint.sh /entrypoint.sh

RUN chmod 777 /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["/etc/docker/registry/config.yml"]