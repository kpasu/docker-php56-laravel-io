FROM centos:7.4.1708

RUN yum update -y
RUN yum install -y less wget zip unzip git rsync

RUN yum install -y epel-release
RUN rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

RUN yum install -y httpd redis

# php
RUN yum install --enablerepo=remi,remi-php56 -y install php php-cli php-devel php-common php-mbstring php-mysql php-phpunit-PHPUnit php-pecl-xdebug php-fpm php-gd php-gmp php-mcrypt php-opcache php-pdo php-xml php-zip

# composer
WORKDIR /tmp
RUN curl -sS https://getcomposer.org/installer | php
RUN mv ./composer.phar /usr/local/bin/composer
RUN composer global require "laravel/installer"
ENV PATH $PATH:/root/.composer/vendor/bin

# gulp
RUN yum install -y nodejs libnotify
RUN npm install --g gulp-cli

# etc
RUN yum clean all
RUN /bin/cp -f /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# src init
RUN mkdir -p /var/www/html/laravel/.git
COPY ./src/laravel/.git /var/www/html/laravel/.git/
WORKDIR /var/www/html/laravel
RUN git reset --hard HEAD
COPY ./files/.env /var/www/html/laravel/
RUN composer install
RUN npm install
RUN chown -R apache:apache /var/www/html/laravel
RUN chmod -R 755 /var/www/html/laravel
