FROM docker:18

RUN set -ex \
    && apk add --no-cache apache2-utils

COPY ./files/registry /bin/registry
COPY ./files/config-example.yml /etc/docker/registry/config.yml

VOLUME ["/var/lib/registry"]
EXPOSE 5000

COPY ./files/docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

CMD ["/etc/docker/registry/config.yml"]

RUN set -x && \
    docker pull busybox && \
    docker tag busybox 127.0.0.1:5000/busybox && \
    docker push 127.0.0.1:5000/busybox