Building servo for Android
==
#### Mar 7, 2017 

I will show you step by step below.


## References

- [Building for Android · servo/servo Wiki](https://github.com/servo/servo/wiki/Building-for-Android)
- [servo/README.md · The Servo Parallel Browser Engine Project](https://github.com/servo/servo/blob/master/README.md)
- [Building servo for Android · Jasmine "lnishan" Chen](https://lnishan.github.io/2017/03/07/building-servo-for-android/)

## 1. Setup Virtual Machine

Setup Ubuntu 16.04 with 4G RAM & 30GB memory.

Lessons I learned :

- A minimum of 3GB of RAM is required.
- I ran out of memory space when I was building, so I resize the virtual disk with [resizing virtual disk](http://askubuntu.com/questions/101715/resizing-virtual-drive).

## 2. Setting up the environment 

check second reference to select different operating system

**ubuntu**

```bash
sudo apt-get install git curl freeglut3-dev autoconf \
    libfreetype6-dev libgl1-mesa-dri libglib2.0-dev xorg-dev \
    gperf g++ build-essential cmake virtualenv python-pip \
    libssl-dev libbz2-dev libosmesa6-dev libxmu6 libxmu-dev \
    libglu1-mesa-dev libgles2-mesa-dev libegl1-mesa-dev libdbus-1-dev
```
If `virtualenv` does not exist, try `python-virtualenv`.

Lessons I learned :
 - The official build guide names `libssl1.0-dev` which seems to be renamed to `libssl-dev`.
 
## 3. Install Android SDK & NDK

**Download and Install [Android Studio](https://developer.android.com/studio/index.html)**

1. download and unpack .zip file
2. open cmd and `cd /path/to/ android-studio/bin/` 
3. excute `./studio.sh`
4. download and unpack old version sdk tools at the bottom of the android studio websites
5. replace the "tools" directory 

**Download and Install [Android NDK](https://developer.android.com/ndk/downloads/older_releases.html)**

1. choose 12b version
2. download and unpack .zip file

**Set the Environment Variables**

```bash
export ANDROID_SDK="/path/to/ Android/Sdk"
export ANDROID_NDK="/path/to/ android-ndk-r12b"
export PATH=$PATH:$ANDROID_SDK/platform-tools
source ~/.bashrc
```
Lessons I learned :


- Android NDK r12b is required. 
- `ANDROID_SDK` and `ANDROID_NDK` need to be set globally.

## 4. Other necessary 

**Install openjdk8**

```bash
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update 
sudo apt-get install openjdk-8-jdk

# If you have more than one Java versions installed on your system.
# Run below command set the default Java :
sudo update-alternatives --config java

# Type in a number to select a Java version.
# And set default Java Compiler by running :
sudo update-alternatives --config javac

#Then check :
java -version

# It outputs something like this :
openjdk version "1.8.0_01-internal"
OpenJDK Runtime Environment (build 1.8.0_01-internal-b04)
OpenJDK 64-Bit Server VM (build 25.40-b08, mixed mode) 
```

**additional tools**
```bash
# Get complete libraries
cd $ANDROID_SDK
tools/android - update sdk --no-ui --all --filter platform-tools,android-18,build-tools-23.0.3

# Install ant
sudo apt-get install ant
```
Lessons I learned :

- openjdk-8-jdk is required instead of 7.
- Android SDK Tools, Revision 25.3.0 removed `android` tool. So we need to replace tools directory at last step to install additional tools successfully.

## 5. Build & Package
```bash
# Get source
git clone https://github.com/mozilla/servo.git
cd servo

# Make sure these env vars are set:
export ANDROID_SDK="/path/to/ Android/Sdk"
export ANDROID_NDK="/path/to/ android-ndk-r12b"

# Build
# Replace "--release" with "--dev" to create an unoptimized debug build.
./mach build --release --android

# Package
./mach package --release --android
```
If all go smoothly, you will get `servo.apk` inside the `target/arm-linux-androideabi/release/` directory.

---
