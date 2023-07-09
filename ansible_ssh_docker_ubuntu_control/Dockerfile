# Choosen ubuntu 16, since later OS versions not supporting ssh in container
FROM ubuntu:16.04

# install this package if sudo requied add before apt and after RUN
# update the repo and install the ansible
# needed for apt-add-repository command
RUN apt-get update 
# RUN apt-get install sudo 
RUN apt install software-properties-common -y
RUN apt-add-repository ppa:ansible/ansible
RUN apt install ansible -y

# installing the SSH server and setting root user and password
RUN apt-get update && apt-get install -y openssh-server 
RUN mkdir /var/run/sshd
RUN echo 'root:password' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# required for SSH not clear the env variables
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# open the SSH terminal after starting the container
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
