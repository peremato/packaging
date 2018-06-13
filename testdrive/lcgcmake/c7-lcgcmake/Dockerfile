FROM centos:7
MAINTAINER Javier Cervantes <javier.cervantes@cern.ch>

ENV LCGCMAKE_ROOT=/build/lcgcmake            \
    LCGCMAKE_BUILD=/build/lcgcmake-build     \
    LCGCMAKE_INSTALL=/build/lcgcmake-install \
    ARCH=x86_64

# Install base requirements (extracted from Puppet configuration)
RUN yum update -y                           && \
    yum install -y epel-release             && \
    yum update -y                           && \
    yum install -y          \
      autoconf              \
      automake              \
      bzip2                 \
      bzip2-devel           \
      cmake3                \
      curl                  \
      expat                 \
      expat-devel           \
      gdbm-devel            \
      git                   \
      glibc-devel           \
      libcurl-devel         \
      libuuid-devel         \
      libXmu-devel          \
      libXpm-devel          \
      mesa-libGL-devel      \
      mesa-libGLU-devel     \
      openmotif-devel       \
      openssl-devel         \
      readline              \
      readline-devel        \
      systemtap             \
      tk-devel              \
      tkinter               \
      unzip                 \
      wget                  \
      which               &&\
      yum install -y centos-release-scl-rh && \
      yum install -y --setopt=tsflags=nodocs  \
        devtoolset-6-gcc                      \
        devtoolset-6-gcc-c++                  \
        devtoolset-6-gcc-gfortran             \
        devtoolset-6-gdb                   
 

# Setup LCGCMake
RUN mkdir -p $LCGCMAKE_ROOT  && \
    git clone https://gitlab.cern.ch/sft/lcgcmake.git $LCGCMAKE_ROOT && \
    mkdir $LCGCMAKE_BUILD $LCGCMAKE_INSTALL

# Copy heptool file
COPY heptools-hsf.cmake $LCGCMAKE_ROOT/cmake/toolchain

RUN rm -f /run/nologin

RUN rm -rf /root/*.*

RUN useradd hsf

ENV USER=hsf
ENV HOME=/home/hsf
WORKDIR /home/hsf

# Alias for cmake
RUN echo 'alias cmake="cmake3"' >> ~/.bashrc

RUN chown hsf -R /build

USER hsf

CMD ["/bin/bash"]
