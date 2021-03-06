This README file contains information on the contents of the meta-adpdistro layer.

In addition, the "How to Build" section describes all the required steps on a Ubuntu 18 Linux
machine to get a bootable SD-Card image with the ChruselPoky Linux Distribution.

Please see the corresponding sections below for details.

# 1. Dependencies

* URI: https://git.yoctoproject.org/cgit/cgit.cgi/poky/
    * branch: warrior

* URI: https://git.yoctoproject.org/git/meta-raspberrypi
    * branch: warrior

* URI: git://git.openembedded.org/meta-openembedded
    * layers: meta-oe, meta-multimedia, meta-networking, meta-python
    * revision: warrior

Maintainer: Christian Herzig <github.herzig.info>

# 2. How To Build

## 2.1 Prerequisite

Required Tools and Packages:

    sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
    build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
    xz-utils debianutils iputils-ping libsdl1.2-dev xterm bmap-tools

Fetch and Install repo tool:

    mkdir ~/bin
    PATH=~/bin:$PATH

    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo

## 2.2 Get all the Yocto Meta code

    mkdir ~/yocto-rpi
    cd ~/yocto-rpi
    repo init -u git@github.com:chrusel/yocto-manifests.git
    repo sync

## 2.3 Setup environment

    cd ~/yocto-rpi
    source chruselpoky/chruselpoky-init-build-env

# 3. Bake Raspberrypi3 image

## 3.1 Trigger build engine to do so

    bitbake -k core-image-sato

## 3.2 Flash image

**Get the ${DISK} of your SD Card with `sudo fdisk -l` command**

    sudo bmaptool copy tmp/deploy/images/raspberrypi3-64/core-image-sato-raspberrypi3-64.wic /dev/sd${DISK}

# 4. Bake Raspberrypi3 SDK (Cross Toolchain)

## 4.1 Trigger build engine to do so

    bitbake core-image-sato -c populate_sdk

## 4.2 Install SDK

    ./tmp/deploy/sdk/chruselpoky-glibc-x86_64-core-image-sato-aarch64-raspberrypi3-64-toolchain-2.7.sh
