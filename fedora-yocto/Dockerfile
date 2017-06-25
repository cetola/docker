FROM fedora:24

MAINTAINER stephanoc@gmail.com 

RUN dnf -y update
RUN dnf -y install gawk make wget tar bzip2 gzip python3 unzip perl patch \
diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath \
ccache perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue perl-bignum socat \
findutils which file cpio python python3-pip python3-pexpect xz which \
SDL-devel xterm vim hostname iputils procps iproute \

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

# Install repo
RUN mkdir /home/${USERNAME}/bin
RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /home/${USERNAME}/bin/repo
RUN chmod a+x /home/${USERNAME}/bin/repo
RUN touch /home/${USERNAME}/.bashrc
RUN echo "PATH=$PATH:~/bin" >> /home/${USERNAME}/.bashrc

# Disable host checking for github
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> /home/${USERNAME}/.ssh/config
RUN chown $USERNAME:$USERNAME /home/${USERNAME}/.ssh/config

USER $USERNAME
ENV HOME /home/$USERNAME

CMD ["/bin/bash"]
