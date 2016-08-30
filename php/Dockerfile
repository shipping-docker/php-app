FROM ubuntu:16.04

MAINTAINER Chris Fidao

RUN locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \

    && apt-get install -y curl software-properties-common \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get install -y php-fpm php-cli php-mcrypt php-gd php-mysql \
       php-pgsql php-imap php-memcached php-mbstring php-xml \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
ADD www.conf /etc/php/7.0/fpm/pool.d/www.conf

EXPOSE 9000
CMD ["php-fpm7.0"]
