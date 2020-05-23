FROM ubuntu:18.04

# Variables necesarias
ARG git_home_path=/home/git
ARG sophos_home_path=/home/sophos
ARG nombre_repo=producto_web
ARG repo_path=$git_home_path/$nombre_repo.git
ARG sophos_file=sav-linux-free-9
ARG sophos_user=sophos
ARG git_user=git
ARG git_username=davidmifo@gmail.com
ARG git_email="David Miralles"
ARG pub_key=gitkey.pub

# Instalar paquetes necesarios
RUN apt-get update && apt-get install -y nano ssh curl

# Instalar Git server
RUN apt-get update && apt-get install -y git

# Crear usuario para git y sophos
RUN adduser --disabled-password git
RUN adduser --disabled-password sophos

# Configurar GIT SSH Key para acceso
RUN mkdir $git_home_path/.ssh && chmod 700 $git_home_path/.ssh && chown git:git $git_home_path/.ssh
RUN touch $git_home_path/.ssh/authorized_keys && chmod 644 $git_home_path/.ssh/authorized_keys
ADD $pub_key /tmp/
RUN cat /tmp/$pub_key >> $git_home_path/.ssh/authorized_keys

# Configurar SOPHOS SSH Key para acceso
RUN mkdir $sophos_home_path/.ssh && chmod 700 $sophos_home_path/.ssh && chown sophos:sophos $sophos_home_path/.ssh
RUN touch $sophos_home_path/.ssh/authorized_keys && chmod 644 $sophos_home_path/.ssh/authorized_keys
ADD $pub_key /tmp/
RUN cat /tmp/$pub_key >> $sophos_home_path/.ssh/authorized_keys

# Creación y configuración de repositorio
USER git
RUN cd $git_home_path && mkdir $nombre_repo.git && cd $nombre_repo.git && git config --global user.email "$git_username" && git config --global user.name "$git_email"
RUN cd $repo_path && git --bare init && git config receive.denyCurrentBranch updateInstead


# Add git hook
USER root
ADD post-update $repo_path/.git/hooks/
RUN chown git:git $repo_path/.git/hooks/post-update && chmod 755 $repo_path/.git/hooks/post-update

# Initial commit
USER root
WORKDIR $repo_path
COPY web_producto/ .
RUN chown git:git -R $repo_path
RUN git add . && git commit -m "Commit Inicial"

#Enable SSH KEY access on ssh
USER root
RUN echo "PubkeyAuthentication yes" > /etc/ssh/sshd_config

# Instalar Sophos antivirus
ADD $sophos_file.tgz /tmp/

# Abrir puerto para ssh
EXPOSE 22