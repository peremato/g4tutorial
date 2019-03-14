# Geant4 image for tutorials
FROM ubuntu:18.04
MAINTAINER Pere Mato <pere.mato@cern.ch>

ENV LCGCMAKE_ROOT=/usr/local/lcgcmake\
    ARCH=x86_64 \
    DEBIAN_FRONTEND=noninteractive\
    LCG_VERSION=95\
    LCG_PLATFORM=x86_64-ubuntu1804-gcc7-opt

RUN apt-get update && \
    apt-get upgrade -y

# Tools necessary for installing and configuring Ubuntu
RUN apt-get install -y \
    python git cmake nano \
    g++ gcc gfortran binutils uuid-dev libssl-dev curl libdbus-1-3\
    libbz2-dev libx11-dev libxpm-dev libxft-dev libxext-dev libmotif-dev libtiff5-dev \
    mesa-common-dev libglu1-mesa libxi-dev libxmu-dev libglu1-mesa-dev \
    libncurses5-dev zlib1g-dev libcurl4-openssl-dev libreadline6-dev \
    libgdbm-dev tk-dev libgl2ps-dev liblzma-dev

# Setup LCGCMake
RUN git clone -b LCG_${LCG_VERSION} https://gitlab.cern.ch/sft/lcgcmake.git $LCGCMAKE_ROOT
RUN ln -s $LCGCMAKE_ROOT/bin/lcgcmake /usr/local/bin/lcgcmake
RUN chmod +x /usr/local/bin/lcgcmake
RUN echo source /opt/lcg/${LCG_VERSION}/${LCG_PLATFORM}/setup.sh > /etc/profile.d/lcg-setup.sh


# Create user g4
RUN useradd -m g4
ENV USER=g4
ENV HOME=/home/g4
RUN chown g4:g4 -R $LCGCMAKE_ROOT
RUN chown g4:g4 /usr/local/bin/lcgcmake
RUN chown g4:g4 /opt

# Install the all the needed LCG packages
USER g4
RUN mkdir /tmp/build
WORKDIR /tmp/build
RUN lcgcmake config --version ${LCG_VERSION}
RUN lcgcmake install Geant4 CMake ninja
RUN rm -rf /tmp/build

# Geant4 datasets
RUN mkdir -p /opt/lcg/Geant4/share/data
WORKDIR /opt/lcg/Geant4/share/data
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4NDL.4.5.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4EMLOW.7.7.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4PhotonEvaporation.5.3.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4RadioactiveDecay.5.3.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4PARTICLEXS.1.1.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4PII.1.3.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4RealSurface.2.1.1.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4SAIDDATA.2.0.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4ABLA.3.1.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4INCL.1.0.tar.gz | tar xz
RUN curl https://geant4-data.web.cern.ch/geant4-data/datasets/G4ENSDFSTATE.2.2.tar.gz | tar xz
RUN sed -i s:/cvmfs/geant4.cern.ch:/opt/lcg/Geant4:g /opt/lcg/${LCG_VERSION}/${LCG_PLATFORM}/bin/geant4-config
ENV G4INSTALL=/opt/lcg/${LCG_VERSION}/${LCG_PLATFORM}
ENV G4EXAMPLES=${G4INSTALL}/share/Geant4-10.5.0/examples

WORKDIR ${HOME}

