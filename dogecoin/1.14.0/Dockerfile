FROM debian:stretch-slim

RUN groupadd -r dogecoin && useradd -r -m -g dogecoin dogecoin

RUN set -ex \
	&& apt-get update \
	&& apt-get install -qq --no-install-recommends ca-certificates dirmngr gosu gpg wget \
	&& rm -rf /var/lib/apt/lists/*

ENV DOGECOIN_VERSION 1.14.0
ENV DOGECOIN_URL https://github.com/dogecoin/dogecoin/releases/download/v1.14.0/dogecoin-1.14.0-x86_64-linux-gnu.tar.gz
ENV DOGECOIN_SHA256 ed7baccafab98b5ce452bd3fd2cf7ab5e757269105350283e2bab91e4ccb7366
ENV DOGECOIN_DATA /data

# install Dogecoin binaries
RUN set -ex \
	&& cd /tmp \
	&& wget -qO dogecoin.tar.gz "$DOGECOIN_URL" \
	&& echo "$DOGECOIN_SHA256 dogecoin.tar.gz" | sha256sum -c - \
	&& tar -xzvf dogecoin.tar.gz -C /usr/local --strip-components=1 --exclude=*-qt \
	&& rm -rf /tmp/*

# create data directory
RUN mkdir "$DOGECOIN_DATA" \
	&& chown -R dogecoin:dogecoin "$DOGECOIN_DATA" \
	&& ln -sfn "$DOGECOIN_DATA" /home/dogecoin/.dogecoin \
	&& chown -h dogecoin:dogecoin /home/dogecoin/.dogecoin
VOLUME /data

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 5222 5223 25222 25223 25222 25223
CMD ["dogecoind"]

