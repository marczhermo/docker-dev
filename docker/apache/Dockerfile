FROM ubuntu:16.04

MAINTAINER marcz

# Set the locale
RUN apt-get clean && apt-get update
RUN apt-get install locales
RUN locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# To install libapache2-mod-fastcgi
#RUN echo 'deb http://archive.ubuntu.com/ubuntu/ xenial multiverse' >> /etc/apt/sources.list
#RUN echo 'deb http://archive.ubuntu.com/ubuntu/ xenial-updates multiverse' >> /etc/apt/sources.list

RUN apt-get update \
    && apt-get install -y curl zip unzip git software-properties-common \
       dnsutils iputils-ping net-tools sqlite3 \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -qq apache2 libapache2-mod-php5.6 \
       php5.6-cli php5.6-fpm php5.6-mysql php5.6-pgsql php5.6-sqlite \
       php5.6-curl php5.6-gd php5.6-gmp php5.6-mcrypt php5.6-memcached php5.6-imagick \
       php5.6-intl php5.6-xdebug php5.6-tidy php5.6-xml php5.6-mbstring php5.6-dev \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY servername.conf /etc/apache2/conf-available/servername.conf
COPY default.conf /etc/apache2/sites-available/default.conf

COPY date.ini /etc/php/5.6/mods-available/date.ini
COPY xdebug.ini /etc/php/5.6/mods-available/xdebug.ini

# Using libapache2-mod-fastcgi
#RUN a2enmod fastcgi

RUN a2enmod actions alias rewrite
RUN a2enconf servername && a2ensite default && service apache2 restart

# Default command
CMD ["apachectl", "-D", "FOREGROUND"]

EXPOSE 80 9000
