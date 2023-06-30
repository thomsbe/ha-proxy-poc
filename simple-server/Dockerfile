FROM busybox:latest

RUN adduser -D static
USER static
WORKDIR /home/static

CMD ["busybox", "httpd", "-f", "-v", "-p", "3000"]