FROM php:7.2.11-fpm

ARG TIMEZONE
ARG LANGUAGE
ARG LOCALE

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        tzdata \
        locales \
        less \
        mysql-client \
        sudo \
        golang-go \
        git \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install opcache

# System Timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone

# PHP Timezone
RUN printf '[PHP]\ndate.timezone = "'${TIMEZONE}'"\n' > /usr/local/etc/php/conf.d/tzone.ini

# Set Locales
RUN sed -i "s/# ${LOCALE}.UTF-8 UTF-8/${LOCALE}.UTF-8 UTF-8/" /etc/locale.gen && locale-gen

ENV LANG ${LOCALE}.UTF-8
ENV LANGUAGE ${LOCALE}:${LANGUAGE}
ENV LC_ALL ${LOCALE}.UTF-8

# User ID & Group ID
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# Add WP-CLI
RUN curl -o /bin/wp-cli.phar https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
COPY ./wp-su.sh /bin/wp
RUN chmod a+x /bin/wp-cli.phar /bin/wp
RUN mkdir /var/www/.wp-cli && chown www-data:www-data /var/www/.wp-cli

# Mailhog
ENV GOPATH /go
RUN go get github.com/mailhog/mhsendmail
RUN cp /go/bin/mhsendmail /bin/mhsendmail
RUN echo 'sendmail_path = /bin/mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/conf.d/mailhog.ini

# Custom Prompt .bashrc
RUN echo "PS1='\[\033[1;36m\][\u@\h:\[\033[1;34m\]\w\[\033[0;35m\]\[\033[1;36m\]]# \[\033[0m\]'" > /root/.bashrc
