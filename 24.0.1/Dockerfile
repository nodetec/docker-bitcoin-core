FROM debian:bullseye-slim

RUN useradd -r bitcoin \
  && apt-get update -y \
  && apt-get install -y curl gnupg gosu \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ARG TARGETPLATFORM
ENV BITCOIN_VERSION=24.0.1
ENV BITCOIN_DATA=/home/bitcoin/.bitcoin
ENV PATH=/opt/bitcoin-${BITCOIN_VERSION}/bin:$PATH

RUN curl -SL https://raw.githubusercontent.com/Kvaciral/kvaciral/main/kvaciral.asc | gpg --import

RUN set -ex \
  && curl -SLO https://bitcoincore.org/bin/bitcoin-core-24.0.1/bitcoin-24.0.1-x86_64-linux-gnu.tar.gz

RUN set -ex \
  && tar -xzf *.tar.gz -C /opt

RUN rm *.tar.gz

RUN rm -rf /opt/bitcoin-${BITCOIN_VERSION}/bin/bitcoin-qt

COPY docker-entrypoint.sh /entrypoint.sh

RUN chmod +x /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18443 18444 38333 38332

ENTRYPOINT ["/entrypoint.sh"]

RUN bitcoind -version | grep "Bitcoin Core version v${BITCOIN_VERSION}"

CMD ["bitcoind"]
