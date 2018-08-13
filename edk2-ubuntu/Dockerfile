FROM ubuntu:16.04

MAINTAINER stephano.cetola@linux.intel.com

RUN apt-get update && apt-get install -y build-essential git vim nasm wget m4 bison flex uuid-dev python unzip qemu

# Add a known password
RUN echo 'root:password' | chpasswd

# setup env
ENV USERNAME dockeruser 
ENV HOMEDIR /home/${USERNAME}
ENV BINDIR ${HOMEDIR}/bin
ENV TMPDIR ${HOMEDIR}/tmp
ENV ACPICA acpica-unix-20180629
ENV MINNOWOBJ minnowboard_max-0.98-binary.objects.zip
ENV WORKDIR ${HOMEDIR}/workspace
ENV MINNOWBIN ${HOMEDIR}/silicon

# create dirs
RUN useradd -ms /bin/bash $USERNAME
RUN echo "$USERNAME:password" | chpasswd
RUN mkdir -p ${WORKDIR}
RUN mkdir -p ${MINNOWBIN}
RUN mkdir -p ${BINDIR}
RUN mkdir -p ${TMPDIR}
RUN chown -R $USERNAME:$USERNAME ${HOMEDIR}
RUN chown -R $USERNAME:$USERNAME ${BINDIR}
RUN chown -R $USERNAME:$USERNAME ${TMPDIR}
RUN chown -R $USERNAME:$USERNAME ${MINNOWBIN}
RUN chown -R $USERNAME:$USERNAME ${WORKDIR}

# get the ASL compiler and minnow binaries
RUN wget https://acpica.org/sites/acpica/files/${ACPICA}.tar.gz -P ${TMPDIR}
RUN tar -xf ${TMPDIR}/${ACPICA}.tar.gz -C ${TMPDIR}
RUN wget https://firmware.intel.com/sites/default/files/${MINNOWOBJ} -O ${TMPDIR}/minnow-bin.zip
RUN unzip ${TMPDIR}/minnow-bin.zip -d ${TMPDIR}
RUN chown -R $USERNAME:$USERNAME ${TMPDIR}
RUN mv ${TMPDIR}/Minnow*/* ${MINNOWBIN}
RUN chown -R $USERNAME:$USERNAME ${MINNOWBIN}

# compile the ASL compiler and tools
WORKDIR ${TMPDIR}/${ACPICA}
RUN make
RUN PREFIX=${HOMEDIR} make install
RUN chown -R $USERNAME:$USERNAME ${BINDIR}

# add $BINDIR to $PATH and change $PS1
RUN touch ${HOMEDIR}/.bashrc
RUN echo "PATH=$PATH:~/bin" >> ${HOMEDIR}/.bashrc
RUN echo 'export PS1="[\u@edk2-ubuntu] \W # "' >> ${HOMEDIR}/.bashrc

# Make ssh dir
RUN mkdir ${HOMEDIR}/.ssh/

# Disable host checking for github
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> ${HOMEDIR}/.ssh/config
RUN chown $USERNAME:$USERNAME ${HOMEDIR}/.ssh/config

USER $USERNAME
ENV HOME ${HOMEDIR}
WORKDIR ${WORKDIR}
CMD ["/bin/bash"]
