FROM pimcore/pimcore:PHP7.4-fpm-debug

ENV DOCKER_IMAGE php-fpm
ENV DOCKER_NAME php-fpm

RUN set -x && \
  rm -rf /usr/local/etc/php-fpm.d

RUN set -x && \
    apt-get update && \
    apt-get install --no-install-recommends --no-install-suggests -y \
        apt-transport-https ca-certificates && \
    apt-get install -y \
        cron \
        curl \
        net-tools \
        vim-tiny \
        procps \
        libcurl4-openssl-dev \
        libfreetype6-dev \
        libicu-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libonig-dev \
        libpng-dev \
        libwebp-dev \
        libxslt1-dev \
        default-mysql-client \
        libzip-dev \
        zip \
        unzip \
        git \
        pv \
        iproute2 iputils-ping && \
    /usr/bin/install -m 0777 -o www-data -g www-data -d /var/www/.composer && \
    apt-get purge -y --auto-remove

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer

VOLUME /var/www/.composer

ENV COMPOSER_CACHE_DIR=/var/www/.composer

#green terminal size
ENV COLUMNS="`tput cols`"
ENV LINES="`tput lines`"

#copy php-fpm and other configuration
COPY ./conf /usr/local/etc/

# env php.ini
ENV PHP_MEMORY_LIMIT "2G"
ENV PHP_MAX_EXECUTION_TIME "30"
ENV PHP_DATE_TIMEZONE "UTC"
ENV PHP_UPLOAD_MAX_FILESIZE "20M"
ENV PHP_POST_MAX_SIZE "20M"
ENV PHP_ERROR_REPORTING "E_ALL & ~E_DEPRECATED & ~E_STRICT"
ENV PHP_DISPLAY_STARTUP_ERRORS "Off"

# opcache settings
ENV OPCACHE_ENABLE 1
ENV OPCACHE_ENABLE_CLI 0
ENV OPCACHE_MEMORY_CONSUMPTION 512
ENV OPCACHE_MAX_ACCELERATED_FILES 100000
ENV OPCACHE_VALIDATE_TIMESTAMPS 1
ENV OPCACHE_CONSISTENCY_CHECKS 0

# env php-fpm.ini
RUN mkdir -p /var/log/php/;
RUN chmod -R o+w /var/log/php;

ENV PHP_FPM_LOG_ERRORS 1
# ENV PHP_FPM_DISPLAY_ERRORS 1 # must be empty to have them off, 0 does not work!
ENV PHP_FPM_PORT 9000
ENV PHP_FPM_ACCESS_LOG="/var/log/php/fpm-access.log"
ENV PHP_FPM_ERROR_LOG="/proc/self/fd/2"
ENV PHP_FPM_PM dynamic
ENV PHP_FPM_PM_MAX_CHILDREN 50
ENV PHP_FPM_PM_START_SERVERS 10
ENV PHP_FPM_PM_MAX_SPARE_SERVERS 20
ENV PHP_FPM_PM_MIN_SPARE_SERVERS 5
ENV PHP_FPM_CATCH_WORKERS_OUTPUT 1
ENV PHP_FPM_PM_MAX_REQUESTS 10000

# add user
ENV BUILD_USER=1000
RUN useradd -d /home/pimcore -m -u $BUILD_USER -g www-data pimcore && \
    echo ". /etc/bash.bashrc" >> /home/pimcore/.bashrc && \
    echo "\
    alias ll='ls -lh --color' \n\
    alias vim='vim.tiny' \n\
    if [[ -v PROJECT_PATH && -d \"\$PROJECT_PATH\" ]]; then cd \"\$PROJECT_PATH\"; fi \n\
    " >> /etc/bash.bashrc 

