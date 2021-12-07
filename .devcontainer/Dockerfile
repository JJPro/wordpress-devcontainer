# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.205.2/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version (use bullseye or stretch on local arm64/Apple Silicon): bullseye, buster, stretch
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
ARG PHP_VERSION="7.4"

COPY .zshrc /home/vscode/.zshrc

# nmap: for network diagnosis 
# ghostscript: required for rendering PDF previews
# php-dev php-pear: installs pecl (PHP extension community library)
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
                            make \
                            nmap \
                            iputils-ping \
                            libfreetype6-dev \
                            libjpeg-dev \
                            libmagickwand-dev \
                            libpng-dev \
                            libwebp-dev \
                            libzip-dev \
                            mariadb-client \
                            php${PHP_VERSION}-common php${PHP_VERSION}-fpm php${PHP_VERSION}-cli php${PHP_VERSION}-curl php${PHP_VERSION}-mysql php${PHP_VERSION}-mbstring php${PHP_VERSION}-gd php${PHP_VERSION}-bcmath php${PHP_VERSION}-zip php${PHP_VERSION}-xml php${PHP_VERSION} php-xdebug \
                            composer \
                            ghostscript \
                            php${PHP_VERSION}-dev php-pear \
    && yes '' | pecl install imagick \
    && apt purge -y --auto-remove
    
COPY 20-xdebug.ini 20-imagick.ini /etc/php/${PHP_VERSION}/cli/conf.d/

# install wp-cli
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && chmod +x wp-cli.phar && mv wp-cli.phar /usr/local/bin/wp

COPY wait-for-it.sh /wait-for-it.sh
RUN chmod +x /wait-for-it.sh

# download wp core for includePath setting of workspace 
USER vscode
WORKDIR /home/vscode/wp
RUN wp core download && \
    wp config create --dbhost=127.0.0.1 --dbname=wordpress --dbuser=wordpress --dbpass=wordpress --dbcharset=utf8mb4 --skip-check --extra-php="define( 'WP_DEBUG', true);"
    