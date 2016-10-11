---
layout: section
order: 3
title: Installing SBT
---

Sbt is the interactive build tool for Scala projects, used to manage dependencies and tasks in projects.

## Ubuntu

```sh
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
sudo apt-get update
sudo apt-get install sbt
```

## MacOS



```sh
brew install sbt   
```

OR

```sh
port install sbt
```

## Windows

Download and install the [msi](https://dl.bintray.com/sbt/native-packages/sbt/0.13.12/sbt-0.13.12.msi) installer.
