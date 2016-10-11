---
layout: section
order: 1
title: Installing Java
---

- TOC
{:toc}

You can either install **Oracle Java** or **OpenJDK** (the Open Source implementation of Oracle Java)

## Installing Oracle Java

### Ubuntu

```sh
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### MacOS (via Homebrew)

```sh
brew update
brew tap caskroom/cask
brew install Caskroom/cask/java
```

### Windows
Follow the instructions [here](https://www.java.com/en/download/help/download_options.xml) and then set the [JAVA_HOME environment variable](https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html)


## Installing OpenJDK

### Ubuntu

```sh
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```

### MacOS

[Instructions on building OpenJDK](https://github.com/manasthakur/tech/wiki/Building-OpenJDK-8-on-Mac-OS-X-Yosemite)

### Windows
Download and run the [MSI-based installer](https://developers.redhat.com/download-manager/file/java-1.8.0-openjdk-1.8.0.102-1-redhat.b14.windows.x86_64.msi) then follow the installation instructions on the screen.
