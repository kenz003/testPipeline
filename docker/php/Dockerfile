# Base image
FROM php:8.2.10-fpm

ARG UID
ARG GID

# Using ARGs to set environment variables
ENV UID=${UID}
ENV GID=${GID}

# both the group and user creation steps in your Dockerfile handle pre-existing conditions
RUN if ! getent group ${GID} ; then addgroup --system --gid ${GID} learnfinitylabs; fi
RUN if ! id -u ${UID} >/dev/null 2>&1; then \
      adduser --system --uid ${UID} --ingroup learnfinitylabs --disabled-login --disabled-password --shell /bin/sh --no-create-home learnfinitylabs; \
    else \
      echo "User with UID ${UID} already exists"; \
    fi

# Update www.conf to use the custom user and group
RUN sed -i 's/user = www-data/user = learnfinitylabs/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/group = www-data/group = learnfinitylabs/g' /usr/local/etc/php-fpm.d/www.conf

# Downloading and setting permissions for the PHP extensions installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions @composer pdo_pgsql

WORKDIR ${PHP_HOST_ROOT_DIRECTORY}

COPY docker/php/php.ini /usr/local/etc/php/php.ini


USER learnfinitylabs

