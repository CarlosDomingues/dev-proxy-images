ARG DEBIAN_VERSION=bookworm-slim
FROM debian:${DEBIAN_VERSION}

ENV DEBIAN_FRONTEND=noninteractive

ARG DEVPROXY_VERSION=v0.21.0

RUN apt-get update \
    && apt-get install --yes curl unzip libicu-dev \
    && curl -sL https://raw.githubusercontent.com/microsoft/dev-proxy/${DEVPROXY_VERSION}/scripts/setup.sh | bash \
    && apt-get remove --purge --yes curl unzip \
    && apt-get autoremove --yes \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN groupadd --gid 1001 devproxygroup \
    && useradd --uid 1001 --gid devproxygroup --shell /bin/bash --create-home devproxyuser \
    && chown -R devproxyuser:devproxygroup /devproxy /home/devproxyuser

USER devproxyuser
RUN /devproxy/devproxy msgraphdb

ENV DEVPROXY_PORT=8000
ENV DEVPROXY_PRESET=/devproxy/presets/m365.json
ENV DEV_PROXY_LOG_LEVEL=Information

EXPOSE ${DEVPROXY_PORT}
EXPOSE 8897

CMD ["bash", "-c", "/devproxy/devproxy", "--config-file", "${DEVPROXY_PRESET}", "--ip-address", "0.0.0.0", "--port", "${DEVPROXY_PORT}", "--log-level", "${DEV_PROXY_LOG_LEVEL}"]
