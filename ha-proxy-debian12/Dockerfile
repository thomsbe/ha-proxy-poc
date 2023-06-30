FROM debian:11.0-slim

RUN apt-get update && apt-get install -y haproxy netcat-openbsd socat && rm -rf /var/lib/apt/lists/*

CMD ["haproxy", "-f", "/usr/local/etc/haproxy/haproxy.cfg"]