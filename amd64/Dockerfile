FROM amd64/alpine:3.8

ENV VERSION 0.6.2

COPY files/ /

RUN set -x; \
    GPGKEY='3C2F2605E078A1E18F4793909C4DBE6CF438F333' \
    && apk add --no-cache --virtual .build-deps gnupg \
    && wget https://github.com/lukas2511/dehydrated/releases/download/v$VERSION/dehydrated-$VERSION.tar.gz -O dehydrated.tar.gz \
    && wget https://github.com/lukas2511/dehydrated/releases/download/v$VERSION/dehydrated-$VERSION.tar.gz.asc -O dehydrated.tar.gz.asc \
    && export GNUPGHOME="$(mktemp -d)" \
    && found=''; \
    for server in \
        ha.pool.sks-keyservers.net \
        hkp://keyserver.ubuntu.com:80 \
        hkp://p80.pool.sks-keyservers.net:80 \
        pgp.mit.edu \
    ; do \
        echo "Fetching GPG key $GPGKEY from $server"; \
        gpg --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$GPGKEY" && found=yes && break; \
    done; \
    test -z "$found" && echo >&2 "error: failed to fetch GPG key $GPGKEY" && exit 1; \
    gpg --batch --verify dehydrated.tar.gz.asc dehydrated.tar.gz \
    && tar -xzf dehydrated.tar.gz --strip-components 1 dehydrated-$VERSION/dehydrated -C /usr/bin \
    && mv /dehydrated/run-dehydrated /etc/periodic/daily/ \
    && rm -rf dehydrated.tar.gz dehydrated.tar.gz.asc "$GNUPGHOME" \
    && apk del .build-deps \
    && apk add --no-cache bash curl libressl sudo \
    && addgroup -S dehydrated \
    && adduser -D -S -H -h /dehydrated -s /sbin/nologin -G dehydrated -g dehydrated dehydrated \
    && install -d -o dehydrated -g dehydrated /dehydrated/data /dehydrated/certs /dehydrated/www

VOLUME ["/dehydrated/certs", "/dehydrated/data", "/dehydrated/www"]

CMD ["/dehydrated/docker-entrypoint"]
