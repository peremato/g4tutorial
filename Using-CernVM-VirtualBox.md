# How to use CernVM and VirtualBox as a  Geant4 Tutorial Image

Here are the instructions on how to create a VM for VirtualBox to be able to run the Geant4 tutorials. The basic VM is based on [CernVM](http://cernvm.cern.ch) (~20MB) and makes use of the CVMFS file system to get all system and application software.

## Prerequisites
- You need to need to install **VirtualBox** 6.1 that can be downloaded from [here](https://www.virtualbox.org/wiki/Downloads) for your operating system.
- Install the **cernvm-launch** utility. The instructions are dependent of the host OS:
  - for Linux/MacOS execute the following commands:
    ```
    if [ "x$(uname -s)" = "xLinux" ]; then
      curl -o cernvm-launch https://ecsft.cern.ch/dist/cernvm/launch/bin/Linux/cernvm-launch
    else
      curl -o cernvm-launch https://ecsft.cern.ch/dist/cernvm/launch/bin/Mac/cernvm-launch
    fi
    chmod +x cernvm-launch  # make it executable
    # As root user, place it in a directory from the $PATH environment
    sudo cp cernvm-launch /usr/local/bin
    ```
  - for Windows:
     Download the [cernvm-launch](https://ecsft.cern.ch/dist/cernvm/launch/bin/Win/cernvm-launch.exe) executable.
- On MacOS and using the ssh login interface you may need to install **XQuartz** for using the Geant4 graphics. XQuartz can be downloded from [here](https://www.xquartz.org)

## Create the VM Image
- Clone the repository with the public CernVM contexts
  ```
  git clone https://github.com/cernvm/public-contexts
  ```
- Create VM image
  ```
  cernvm-launch create --name g4-tutorial --cpus 4 --memory 8000 --disk 20000 public-contexts/g4-tutorial.context
  ```
  This will create the image and start the virtual machine

Please note that the first time boot and the first time executing anything in the created image will take some time (minutes) because the CVMFS caches need to be filled. Subsequent reboots and executions will be much faster. 

## Using the VM with ssh login
The VM can be started and stopped using the following commands of `cernvm-launch`:
```
cernvm-launch start g4-tutorial
cernvm-launch stop g4-tutorial
```
### Remote login with ssh
You can remote login to the VM with the following command if the VM is running 
```
cernvm-launch ssh g4-tutorial
```
user:g4user  
password:pass

## Using the graphical desktop of the VM
The VM can also be used from the desktop directly with the necessary conversion of keystrokes between systems MacOS/Win to Linux

### On MacOS you may need to change the graphics controller
- Shutdown the **g4-tutorial** virtual machine
- In the **VirtualBox.app** select g4-tutorial and click on `Settings` icon
- In the `Display` panel select `VMSVGA` controller and enable 3D accelerator
- Re-start the machine

### Change the display resolution
- With a running **g4-tutorial** virtual machine, click the `Settings Manager` icon (most right icon in the bottom icon box) in the desktop
- Select `Display` panel
- Select the desired resolution and `Apply`

## Build and run a basic example
Open a terminal (or ssh to the VM) and execute the following commands. The first time a terminal is open, CVMFS is accessed to setup a full environment, and this takes a bit of time.
```
    cp -r $G4EXAMPLES/basic/B1 .
    cd B1
    mkdir build; cd build
    cmake ..
    make -j4
    ./exampleB1
```
