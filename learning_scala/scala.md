---
layout: section
order: 2
title: Installing Scala
---


## Ubuntu

```bash
wget www.scala-lang.org/files/archive/scala-2.11.8.deb
sudo dpkg -i scala-2.11.8.deb
```

## MacOs (with Homebrew)

```sh
 brew update
 brew install scala
```

## Windows

- Download .msi installer file from Scala Downloads page
- Install Scala by double clicking .msi installer
- This will install Scala on `C:\Program Files (x86)\scala` Or `C:\Program Files\scala` path
- Go to My `Comupter > Properties > Advance System Settings > Environment Variables`
- Create SCALA_HOME variable with the path where Scala is extracted
- In our case it will be `C:\Program Files\scala`
- Update/Create PATH variable and append ``;%SCALA_HOME%/bin;`` to the value
