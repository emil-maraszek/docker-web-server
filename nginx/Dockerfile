
FROM nginx:1.15.12

COPY image-files/ /

COPY docker-nginx-entrypoint /usr/local/bin
RUN chmod 700 /usr/local/bin/docker-nginx-entrypoint

ENTRYPOINT ["docker-nginx-entrypoint"]

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get -y install \
        vim

RUN mkdir /etc/nginx/conf.d-dist

WORKDIR /etc/nginx

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]