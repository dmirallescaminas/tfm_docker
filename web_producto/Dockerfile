FROM ubuntu:18.04

# Variables necesarias
ARG NGINX_PATH=/etc/nginx/sites-available
ARG WEB_PATH=/var/www/html
ARG GIT_SERVER_URL=ec2-3-23-126-105.us-east-2.compute.amazonaws.com:2222
ARG WEB_USER_PATH=/home/web
ARG PUB_KEY=gitkey.pub
ARG PRIV_KEY=gitkey.ppk

# Instalar paquetes necesarios
USER root
RUN adduser --disabled-password web
RUN apt-get update
RUN apt-get install -y putty-tools nano git ssh nginx

# Añadir default custom
RUN rm -f $NGINX_PATH/default
ADD default $NGINX_PATH/

# Añadir public key para recibir conexion
RUN mkdir $WEB_USER_PATH/.ssh && chmod 700 $WEB_USER_PATH/.ssh && chown web:web $WEB_USER_PATH/.ssh
RUN touch $WEB_USER_PATH/.ssh/authorized_keys && chmod 644 $WEB_USER_PATH/.ssh/authorized_keys
ADD $pub_key /tmp/
RUN cat /tmp/$PUB_KEY >> $WEB_USER_PATH/.ssh/authorized_keys

USER root
RUN chmod 777 $WEB_PATH

# Añadir private key para hacer conexion a git
USER web
ADD gitkey.ppk /tmp/
RUN puttygen /tmp/$PRIV_KEY -O private-openssh -o /tmp/gitkeyok
RUN mv /tmp/gitkeyok $WEB_USER_PATH/.ssh/

#Enable SSH KEY access on ssh
USER root
RUN echo "PubkeyAuthentication yes" > /etc/ssh/sshd_config

EXPOSE 80 22