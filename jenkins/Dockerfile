FROM ubuntu:18.04

# Variables needed
ARG user=jenkins
ARG home_path=/home/jenkins
ARG priv_key=gitkey.ppk

USER root
RUN apt-get update
RUN apt-get install -y gnupg gnupg2 gnupg1 wget openjdk-11-jdk putty-tools
RUN apt-get install -y nano ssh curl git apt-utils nodejs

# Install Jenkins
RUN wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | apt-key add -
RUN echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list
RUN apt-get update
RUN apt-get install -y jenkins

# Add pubkey for jenkins user
RUN mkdir $home_path && mkdir $home_path/.ssh && chmod 700 $home_path/.ssh && chown jenkins:jenkins $home_path/.ssh
RUN touch $home_path/.ssh/authorized_keys && chmod 644 $home_path/.ssh/authorized_keys
ADD gitkey.ppk /tmp/
RUN puttygen /tmp/gitkey.ppk -O private-openssh -o gitkey
RUN cp /tmp/gitkey /home/jenkins/.ssh && chown jenkins:jenkins /home/jenkins/.ssh/gitkey

EXPOSE 8080 50000 22
