FROM alpine:latest

# Edge Repository für PHP 8 auf 32-bit hinzufügen
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories

RUN apk update && apk add \
    php83 \
    php83-fpm \
    php83-mysqli \
    php83-pdo_mysql \
    php83-pgsql \
    php83-pdo_pgsql \
    php83-json \
    php83-curl \
    php83-mbstring \
    php83-xml \
    php83-pdo \
    php83-session \
    nginx

# Nginx Config kopieren
COPY nginx.conf /etc/nginx/http.d/default.conf

# PHP-FPM anpassen
RUN sed -i 's/127.0.0.1:9000/9000/' /etc/php83/php-fpm.d/www.conf

# PHP Konfiguration - EINFACHE Version
RUN echo 'error_reporting=E_ALL' >> /etc/php83/conf.d/99-docker.ini && \
    echo 'display_errors=stderr' >> /etc/php83/conf.d/99-docker.ini && \
    echo 'display_startup_errors=On' >> /etc/php83/conf.d/99-docker.ini && \
    echo 'log_errors=On' >> /etc/php83/conf.d/99-docker.ini && \
    echo 'error_log=/dev/stderr' >> /etc/php83/conf.d/99-docker.ini

WORKDIR /var/www/html

CMD ["sh", "-c", "ln -sf /dev/stderr /var/log/nginx/error.log && ln -sf /dev/stdout /var/log/nginx/access.log && php-fpm83 && nginx -g 'daemon off;'"]
