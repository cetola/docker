FROM ubuntu:16.04

MAINTAINER stephanoc@gmail.com

RUN apt-get update && apt-get upgrade -y 
RUN apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev xterm vim cpio openssh-server ncurses-dev parted genext2fs bc locales-all curl gperf texinfo libncurses5-dev python3

# Add a known password
RUN echo 'root:password' | chpasswd

## change username
ENV USERNAME stephano 
RUN useradd -ms /bin/bash $USERNAME
RUN echo "$USERNAME:password" | chpasswd
RUN mkdir -p /opt/yocto/downloads
RUN mkdir -p /opt/yocto/sstate-cache
RUN chown -R $USERNAME:$USERNAME /home/$USERNAME
RUN chown -R $USERNAME:$USERNAME /opt/yocto/downloads
RUN chown -R $USERNAME:$USERNAME /opt/yocto/sstate-cache

# Make ssh dir
RUN mkdir /home/${USERNAME}/.ssh/

# Disable host checking for github
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> /home/${USERNAME}/.ssh/config
RUN chown $USERNAME:$USERNAME /home/${USERNAME}/.ssh/config

USER $USERNAME
ENV HOME /home/$USERNAME

CMD ["/bin/bash"]
