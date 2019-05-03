FROM php:alpine as phpbuilder
WORKDIR /app

RUN apk update && apk upgrade && apk add --no-cache bash git openssh
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

COPY ./ /app
RUN composer install

FROM node:latest as nodebuilder
WORKDIR /app
COPY --from=phpbuilder /app /app
RUN yarn install
RUN yarn run prod

FROM busybox:latest
VOLUME /var/www
COPY --from=nodebuilder /app /var/www/sample
RUN chmod 777 -R /var/www/sample/storage/
CMD tail -f /dev/null
