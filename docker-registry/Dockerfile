FROM docker:edge-dind

RUN set -ex \
    && apk add --no-cache apache2-utils git openssh-client

COPY ./files/registry /bin/registry
COPY ./files/config-example.yml /etc/docker/registry/config.yml

VOLUME ["/var/lib/registry"]
EXPOSE 5000

COPY ./files/docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

CMD ["/etc/docker/registry/config.yml"]