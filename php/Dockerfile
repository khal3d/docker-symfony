FROM php:7.4-fpm

RUN apt-get update && \
    apt-get install -y git zip unzip zlib1g-dev libicu-dev \
    libcurl4-openssl-dev pkg-config libssl-dev \
    && docker-php-ext-install intl pdo_mysql && docker-php-ext-enable intl pdo_mysql \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash && \
    mv /root/.symfony/bin/symfony /usr/local/bin/symfony && \
    adduser --force-badname --disabled-password --gecos '' _www

# Install PHP Composer & PHP CS-Fixer
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer\
    && composer global require friendsofphp/php-cs-fixer
ENV PATH $PATH:/root/.composer/vendor/bin


RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -\
    && apt-get install -y nodejs

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -\
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list\
    && apt-get update && apt-get install -y nodejs yarn

RUN apt-get update && apt-get install -y yarn

VOLUME ["/var/www/html"]
WORKDIR /var/www/html

expose 80

CMD ["symfony", "serve", "--port=80"]
