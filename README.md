# Geant4 Tutorial Container Image

This project is to build an Docker image for running the exercises in Geant4 tutorials and courses. It is based in the CentOS 7 image and it add all the necessary system bits and installs on top the software integrated by the LCG releases. It should be self-contained and not require any remote access for running it.

See the [pre-requisites](#Installation-pre-requisites) depending on your host.

# To use the image (with graphics enabled)

    xhost + 127.0.0.1
    docker run -e DISPLAY=host.docker.internal:0 \
           -e LIBGL_ALWAYS_INDIRECT=1 \
           -it peremato/g4tutorial-centos7:latest \
           bash --login

## Cross-mount your host volumes into the docker image
In this way your work stays outside the image and is not lost when the image is stopped

    docker run -e DISPLAY=host.docker.internal:0 \
           -v $HOME:/home/g4/host \ 
           -it peremato/g4tutorial-centos7 \
           bash --login

Inside the image your host files are under $HOME/host

## Build and run a basic example of Geant4
    cp -r $G4EXAMPLES/basic/B2/B2b .
    mkdir build; cd build
    cmake ../B2b
    make -j4
    ./exampleB2b

# To build and upload the image
First log in to GitLab’s Container Registry using your GitLab username and password. 

    docker login 
    <provide the dockerhub credentials>

Once you log in, you’re free to create and upload a container image using the common build and push commands

    git clone https://gitlab.cern.ch/mato/g4tutorial.git
    docker build -t peremato/g4tutorial-centos7 g4tutorial/centos7
    docker push peremato/g4tutorial-centos7

# Useful docker commands
- stop all running containers  
  `docker rm $(docker ps -a | grep -v Up | cut -d ' ' -f1)`
- prune all unneeded images  
  `docker image prune`


# Installation pre-requisites

## MacOS
You need to install the following packages:
- Docker (see https://docs.docker.com/docker-for-mac/install/)
- XQuatz (see https://www.xquartz.org)  
  In addition, to need to enable `iglx` with the following command:  
  `defaults write org.macosforge.xquartz.X11 enable_iglx -bool true`

## Centos 7






