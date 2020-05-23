FROM ubuntu:18.04

# Variables necesarias
ARG NGINX_PATH=/etc/nginx/sites-available
ARG WEB_PATH=/var/www/html
ARG GIT_SERVER_URL=ec2-3-20-236-170.us-east-2.compute.amazonaws.com:2222

# Instalar paquetes necesarios
USER root
RUN apt-get update
RUN apt-get install -y git ssh nginx #php-fpm

# Añadir default custom
RUN rm -f $NGINX_PATH/default
ADD default $NGINX_PATH/

COPY . $WEB_PATH

EXPOSE 80 22