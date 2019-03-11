# Geant4 Tutorial Container Image

This project is to build an Docker image for running the exercises in Geant4 tutorials and courses. It is based in the Ubuntu LTS 18.04 image and it add all the necessary system bits and installs on top the software integrated by the LCG releases. It should be self-contained and not require any remote access for running it.

# To use the image (with graphics enabled)

    xhost + 127.0.0.1
    docker run -e DISPLAY=host.docker.internal:0 geant4tutorial


# To build and upload the image

    docker build -t geant4tutorial  https:

    

