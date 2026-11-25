FROM ubuntu:24.04

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        asterisk asterisk-modules \
    && rm -rf /var/lib/apt/lists/* \
    && install -d -o asterisk -g asterisk /run/asterisk

CMD ["asterisk", "-f", "-U", "asterisk", "-G", "asterisk"]
