FROM ubuntu:16.04

MAINTAINER marcz

# Set the locale
RUN apt-get clean && apt-get update
RUN apt-get install locales
RUN locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \
    && apt-get install -y curl zip unzip git software-properties-common \
       dnsutils iputils-ping net-tools sqlite3 supervisor nginx \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y php7.1-fpm php7.1-cli php7.1-dev \
       php7.1-pgsql php7.1-sqlite3 php7.1-mcrypt php7.1-gd \
       php7.1-curl php7.1-memcached php7.1-gmp php7.1-tidy \
       php7.1-imap php7.1-mysql php7.1-mbstring \
       php7.1-xml php7.1-zip php7.1-bcmath php7.1-soap \
       php7.1-intl php7.1-readline php7.1-xdebug \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf

COPY default /etc/nginx/sites-available/default
COPY sshost.conf /etc/nginx/sites-available/sshost.conf
COPY php-fpm.conf /etc/php/7.1/fpm/php-fpm.conf
#COPY www.conf /etc/php/7.1/fpm/pool.d/www.conf
COPY date.ini /etc/php/7.1/mods-available/date.ini
COPY xdebug.ini /etc/php/7.1/mods-available/xdebug.ini
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN ln -sf /etc/php/7.1/mods-available/date.ini /etc/php/7.1/fpm/conf.d/10-date.ini
RUN ln -sf /etc/php/7.1/mods-available/xdebug.ini /etc/php/7.1/fpm/conf.d/20-xdebug.ini
RUN ln -sf /etc/nginx/sites-available/sshost.conf /etc/nginx/sites-enabled/sshost.conf

RUN service php7.1-fpm restart

EXPOSE 80

#CMD ["php-fpm7.1"]
#CMD ["nginx"]
CMD ["/usr/bin/supervisord"]
