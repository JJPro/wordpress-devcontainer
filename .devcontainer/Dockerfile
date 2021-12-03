# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.205.2/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version (use bullseye or stretch on local arm64/Apple Silicon): bullseye, buster, stretch
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

COPY .zshrc /home/vscode/.zshrc

# nmap: for network diagnosis 
# ghostscript: required for rendering PDF previews
# php-dev php-pear: installs pecl (PHP extension community library)
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
                            make \
                            nmap \
                            nginx \
                            libfreetype6-dev \
                            libjpeg-dev \
                            libmagickwand-dev \
                            libpng-dev \
                            libwebp-dev \
                            libzip-dev \
                            php-common php-fpm php-cli php-curl php-mysql php-mbstring php-gd php-bcmath php-exif php-zip php-xml php-xdebug php \
                            composer \
                            ghostscript \
                            php-dev php-pear \
    && yes '' | pecl install imagick \
    && apt purge -y --auto-remove
    
COPY 20-xdebug.ini /etc/php/7.4/cli/conf.d/20-xdebug.ini
COPY 20-imagick.ini /etc/php/7.4/cli/conf.d/20-imagick.ini

# TODO set up dev db 
RUN mysql ....db connection.... -e \
            "CREATE DATABASE wordpress; \
            GRANT ALL PRIVILEGES ON wordpress.* TO wpuser@localhost IDENTIFIED BY 'dontlook';"

# download wp core for includePath setting of workspace 
USER vscode
WORKDIR /home/vscode/wp
# TODO change db to db container 
RUN wp core download && \
    wp config create --dbname=wordpress --dbuser=wpuser --dbpass=dontlook --dbcharset=utf8mb4 --skip-check --extra-php="define( 'WP_DEBUG', true);" && \
    wp core install --url=localhost:8000 --title="WP DEV" --admin_user=admin --admin_password=password --admin_email=test@example.com --skip-email

# TODO installed specified plugins and themes 
# TODO config mailhog connection
# TODO set up nginx vhost