---
layout: post
title: 在Ubuntu下面安装RStudio
---

最近把一直在用的Windows 10更换成了Ubuntu，一来是基于UNIX的系统使用各种开发工具更方便，二来是因为要用R，而Windows下面的R对Unicode的支持问题比较多，太麻烦了。特此整理出Ubuntu 16.04 LTS下安装RStudio集成开发环境的方法，方便大家在Ubuntu下使用R这个强大的统计编程语言。

## 安装基础R (Base R)

从Ubuntu的软件中心 （Software Center） 里面是可以直接安装基础R的，但是如果这样安装，R的版本会比较陈旧。最好还是从Cran的服务器来直接安装R。

### 添加R的资源库

我们需要添加Cran的服务器地址到 `/etc/apt/sources.list` 这个系统文件里。使用下面的代码可以直接完成这个操作。注意代码里的“xenial”是对应Ubuntu 16.04的版本代码。如果使用其它版本的Ubuntu，需要更换这个代码。

```bash
sudo echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" | sudo tee -a /etc/apt/sources.list
```

### 把R添加到Ubuntu的Keyring

首先，

```bash
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
```

然后，

```bash
gpg -a --export E084DAB9 | sudo apt-key add -
```

### 安装基础R

```bash
sudo apt-get update
sudo apt-get install r-base r-base-dev
```

## 安装RStudio

可以在[RStudio的网站](https://www.rstudio.com/)下载deb安装包，然后双击deb文件，就能在图形界面安装RStudio了。如果deb安装文件在Ubuntu下面使用比较卡顿，可以尝试下面的方法，用gdebi来安装deb包:

先安装gdebi

```bash
sudo apt-get install gdebi-core
```

然后用gdebi来安装RStudio的deb包，把path/to/your/rstudio/deb更换成RStduio安装包的路径和文件名：

```bash
sudo gdebi -n path/to/your/rstudio/deb
```

接着就可以开始方便地使用RStudio啦。
