# Set up and run a headless Expo development environment

Often Android and IOS development patterns for developing react-native apps follow a localy based graphical UI approach where for example, Android Studio is required to procure the necessary prerequisite tools and libraries to run the development workflow.

This can be a good thing as this gives the developer access to an emulated version of various mobile devices.

However this can be relatively resource intensive and may, at times, slow down the development life cycle.

Running Expo in it's 'web mode' permits for a 'best of both worlds' where Expo serves a url on the development hosts localhost and physical IP address.

Mobile devices with Expo Go pre-installed may connect to this network address and be used as test clients whilst modern and compliant client browsers may be used for rapid ongoing development work.

## Prerequisites

In this example, the target development envrionment is Debian / Ubuntu (.deb package managed) Linux ...

setup an appropriate LTS version of Node - for this see https://github.com/nvm-sh/nvm which has a good explanation of the process to install nvm and apply a version of node with for example:

```
nvm install v14.20.1
nvm use v14.20.1
```

install openjdk with

```
sudo apt install openjdk-11-jre-headless
```
In place of Android Studio, `sdkmanager` is used to install  platform-tools.

```
mkdir ~/Android
cd ~/Android
```
download command line tools from `https://developer.android.com/studio`, using `download options` to find `command-line tools only` and download the zip file appropriate for your platform. In this instance the target envrionment is Linux.

```bash
~/Android$ ls
commandlinetools-linux-8512546_latest.zip
```
extract the command line tools zip file into the `~/Android` directory
```
unzip commandlinetools-linux-8512546_latest.zip
```
to create a `cmdline-tools` directory
```
~/Android$ ls
cmdline-tools  commandlinetools-linux-8512546_latest.zip
```
the zip file may optionally be removed now

from the `~/Android` directory, create a `cmdline-tools/latest` directory and move `commandline-tools` into it
```
~/Android$ mv cmdline-tools/ latest &&  mkdir cmdline-tools && mv latest cmdline-tools/
```
update the path variable to contain the tools we've just downloaded
```
echo "export PATH=$HOME/Android/cmdline-tools/latest/bin:$PATH" | tee -a ~/.bashrc
```
log out / log in again or invoke the `.bashrc` settings in the current session with
```
source ~/.bashrc
```
check `sdkmanager` is working 
```
$ sdkmanager --help
Usage:
  sdkmanager [--uninstall] [<common args>] [--package_file=<file>] [<packages>...]
  sdkmanager --update [<common args>]
  sdkmanager --list [<common args>]
  sdkmanager --list_installed [<common args>]
  sdkmanager --licenses [<common args>]
  sdkmanager --version

...

```
install platform-tools with
```
sdkmanager --install platform-tools
```
typing 'y' + ENTER to accept the terms

the resulting directory structure will now look like
```
~/Android$ tree -L 2
.
├── cmdline-tools
│   └── latest
├── licenses
│   └── android-sdk-license
└── platform-tools
    ├── adb
    ├── dmtracedump
    ├── e2fsdroid
    ├── etc1tool
    ├── fastboot
    ├── hprof-conv
    ├── lib64
    ├── make_f2fs
    ├── make_f2fs_casefold
    ├── mke2fs
    ├── mke2fs.conf
    ├── NOTICE.txt
    ├── package.xml
    ├── sload_f2fs
    ├── source.properties
    └── sqlite3
```

## create react-native app with expo

to create a new `react-native` app with `expo` change directory to be where you want to do this, for example

```
cd ~/projects
npx create-expo-app my-app
```
change to the new app directory and install further pre-requisite packages for web mode with :
```
npx expo install react-native-web@~0.18.7 react-dom@18.0.0 @expo/webpack-config@^0.17.0
```
run the app in web mode with
```
npm run web
```

and then access the url where stated for example :

> Web is waiting on http://localhost:19006

or on an appropriate device having installed 'Expo Go' app at for example

> Metro waiting on exp://192.168.x.x:19000

or just scan the QR code from 'Expo Go' on the mobile device of choice that is already on the local area network of the Metro server above.
