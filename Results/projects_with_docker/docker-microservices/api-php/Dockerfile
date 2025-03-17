FROM alpine:latest

# Install packages
RUN apk --no-cache add php7 php7-fpm php7-pdo php7-pdo_pgsql php7-pgsql php7-json php7-openssl php7-curl \
    php7-zlib php7-simplexml php7-xml php7-phar php7-intl php7-dom php7-xmlreader php7-ctype \
    php7-mbstring php7-gd php7-session php7-tokenizer php7-bcmath php7-fileinfo \
    php7-xmlreader php7-xmlwriter nginx supervisor curl vim bash

# Add composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Configure nginx
COPY ./nginx.conf /etc/nginx/nginx.conf

# Configure supervisord
COPY ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add application
WORKDIR /var/www/html

# Expose the port nginx is reachable on
EXPOSE 8000

# Let supervisord start nginx & php-fpm
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]