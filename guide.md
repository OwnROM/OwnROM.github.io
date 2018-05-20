# OwnROM Build Guide

*Note: This guide is assuming you are running a 64 bit Ubuntu, Debian or Ubuntu / Debian based operating system.*

## 1: Set up your development environment

Make sure your system is up to date:

```bash
$ sudo apt update
$ sudo apt upgrade
```

Install all required build tools:

```bash
$ sudo apt install git-core ninja-build gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 lzop maven pngcrush schedtool lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev squashfs-tools openjdk-8-jre openjdk-8-jdk
```

## 2: Configure Repo and Git

Make a directory for Repo, download it and add it to your path so it can be executed system wide:

```bash
$ mkdir ~/bin
$ PATH=~/bin:$PATH
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
```

Make a folder to store the ROM’s source code (OwnROM can be replaced with whatever you want):

```bash
$ mkdir ~/OwnROM
$ cd ~/OwnROM
```

Configure Git (Replace Your Name and you@example.com with your user name and email for GitHub):

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"
```

## 3: Download the source code

Get the source code initialized:

```bash
$ repo init -u git://github.com/OwnROM/android -b own-o
```

Download the source (replace X with the number of threads you want to use for syncing, this can also take a while depending on your the speed of your internet connection):

```bash
$ repo sync -jX --force-sync
```

## 4: Build the ROM

Load required build tools:

```bash
$ . build/envsetup.sh
```

Fix Java heap error (May or may not be required):

```bash
$ export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4g"
$ ./prebuilts/sdk/tools/jack-admin kill-server
$ ./prebuilts/sdk/tools/jack-admin start-server
```

Build for your device (This can take some time depending on the speed of your computer):

```bash
$ lunch
$ time mka ownrom
```

Between new builds:

```bash
$ make clean
```