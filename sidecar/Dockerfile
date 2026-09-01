FROM docker.elastic.co/beats/filebeat:8.15.3 AS filebeat

FROM ubuntu:jammy

ARG SIDECAR_VERSION=1.5.6

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends ca-certificates curl dpkg \
    && curl -fL --retry 8 --retry-delay 10 --connect-timeout 20 \
        -o /tmp/graylog-sidecar.deb \
        https://github.com/Graylog2/collector-sidecar/releases/download/${SIDECAR_VERSION}/graylog-sidecar_${SIDECAR_VERSION}-1_amd64.deb \
    && dpkg -i /tmp/graylog-sidecar.deb \
    && rm -f /tmp/graylog-sidecar.deb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=filebeat /usr/share/filebeat /usr/share/filebeat
RUN ln -s /usr/share/filebeat/filebeat /usr/bin/filebeat

CMD ["/usr/bin/graylog-sidecar", "-c", "/etc/graylog/sidecar/sidecar.yml"]