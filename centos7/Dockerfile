# Geant4 image for tutorials
FROM centos:7
MAINTAINER Pere Mato <pere.mato@cern.ch>

# Version Geant4 10.6
ENV LCGCMAKE_ROOT=/usr/local/lcgcmake\
    ARCH=x86_64 \
    DEBIAN_FRONTEND=noninteractive \
    LCG_VERSION=hsf \
    LCG_BRANCH=SPI-1518 \
    LCG_COMPILER=gcc8
ENV LCG_PLATFORM=${ARCH}-centos7-${LCG_COMPILER}-opt

RUN yum -y update

# Tools necessary for installing and configuring Ubuntu
RUN yum -y install \
    python git cmake nano which make gcc binutils file sudo readline-devel bzip2-devel libcurl-devel\
    uuid-devel openssl-devel dbus \
    libX11-devel libXpm-devel libXft-devel libXext-devel motif-devel libtiff \
    libXi-devel libXmu-devel mesa-libGL-devel mesa-libGLU-devel glx-utils \
    ncurses-devel zlib-devel \
    gdbm-devel tk-devel xz-devel

# Setup LCGCMake
RUN git clone -b ${LCG_BRANCH} https://gitlab.cern.ch/sft/lcgcmake.git $LCGCMAKE_ROOT &&\
    ln -s $LCGCMAKE_ROOT/bin/lcgcmake /usr/local/bin/lcgcmake &&\
    chmod +x /usr/local/bin/lcgcmake &&\
    echo source /opt/lcg/${LCG_VERSION}/${LCG_PLATFORM}/setup.sh > /etc/profile.d/lcg-setup.sh

# Create user g4
RUN useradd -m -G wheel g4 && echo "g4 ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
ENV USER=g4
ENV HOME=/home/g4
RUN chown g4:g4 -R $LCGCMAKE_ROOT
RUN chown g4:g4 /usr/local/bin/lcgcmake
RUN chown g4:g4 /opt

# Install the all the needed LCG packages
USER g4
RUN mkdir /tmp/build
WORKDIR /tmp/build
RUN lcgcmake config --version ${LCG_VERSION} --compiler ${LCG_COMPILER} &&\
    lcgcmake install Geant4 CMake ninja &&\
    rm -rf /tmp/build

# Geant4 datasets
RUN mkdir -p /opt/lcg/Geant4/share/data
WORKDIR /opt/lcg/Geant4/share/data
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4NDL.4.6.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4EMLOW.7.9.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4PhotonEvaporation.5.5.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4RadioactiveDecay.5.4.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4PARTICLEXS.2.1.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4PII.1.3.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4RealSurface.2.1.1.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4SAIDDATA.2.0.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4ABLA.3.1.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4INCL.1.0.tar.gz | tar xz
RUN curl -s https://geant4-data.web.cern.ch/geant4-data/datasets/G4ENSDFSTATE.2.2.tar.gz | tar xz
RUN sed -i s:/cvmfs/geant4.cern.ch:/opt/lcg/Geant4:g /opt/lcg/${LCG_VERSION}/${LCG_PLATFORM}/bin/geant4-config
ENV G4INSTALL=/opt/lcg/${LCG_VERSION}/${LCG_PLATFORM} \
    G4EXAMPLES=${G4INSTALL}/share/Geant4-10.5.0/examples

WORKDIR ${HOME}

