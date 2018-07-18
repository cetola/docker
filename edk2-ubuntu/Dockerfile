FROM ubuntu:16.04

MAINTAINER stephanoc@gmail.com 

RUN apt-get update && apt-get install -y build-essential git vim nasm wget m4 bison flex uuid-dev python

# Add a known password
RUN echo 'root:password' | chpasswd

# setup env
ENV USERNAME stephano 
ENV HOMEDIR /home/${USERNAME}
ENV BINDIR ${HOMEDIR}/bin
ENV TMPDIR ${HOMEDIR}/tmp
ENV ACPICA acpica-unix-20180629
ENV WORKDIR ${HOMEDIR}/workspace

# create dirs
RUN useradd -ms /bin/bash $USERNAME
RUN echo "$USERNAME:password" | chpasswd
RUN mkdir -p ${WORKDIR}
RUN mkdir ${BINDIR}
RUN mkdir ${TMPDIR}
RUN chown -R $USERNAME:$USERNAME ${HOMEDIR}
RUN chown -R $USERNAME:$USERNAME ${BINDIR}
RUN chown -R $USERNAME:$USERNAME ${TMPDIR}

# get the ASL compiler
RUN wget https://acpica.org/sites/acpica/files/${ACPICA}.tar.gz -P ${TMPDIR}
RUN tar -xf ${TMPDIR}/${ACPICA}.tar.gz -C ${TMPDIR}
RUN chown -R $USERNAME:$USERNAME ${TMPDIR}

# compile the ASL compiler and tools
WORKDIR ${TMPDIR}/${ACPICA}
RUN make
RUN PREFIX=${HOMEDIR} make install
RUN chown -R $USERNAME:$USERNAME ${BINDIR}

# setup BINDIR
RUN touch ${HOMEDIR}/.bashrc
RUN echo "PATH=$PATH:~/bin" >> ${HOMEDIR}/.bashrc

# Make ssh dir
RUN mkdir ${HOMEDIR}/.ssh/

# Disable host checking for github
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> ${HOMEDIR}/.ssh/config
RUN chown $USERNAME:$USERNAME ${HOMEDIR}/.ssh/config

USER $USERNAME
ENV HOME ${HOMEDIR}
WORKDIR ${WORKDIR}
CMD ["/bin/bash"]
