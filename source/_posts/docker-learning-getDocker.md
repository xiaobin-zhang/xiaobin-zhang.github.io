---
title: docker-learning-getDocker
date: 2018-04-08 16:01:23
tags: Docker
categories: 
  - 学习
  - 容器
---
{% asset_img get-docker.jpg %} 

本篇主要的内容是：在linux操作系统下，下载安装Docker。

<!-- more -->

## 概述
    
在CentOS操作系统下，安装Docker。Docker有两个版本，一个是社区版本CE，一个是企业版本EE。本次安装的是社区版本。  
有两种方式来安装Docker，一种是使用YUM repository，一种是使用RPM package。本次使用YUM reppository来安装。

> **Note：**强烈建议使用YUM repository方式来安装Docker，这样便于对Docker的更新和管理。如果是在没有网络连接的环境中，就只能使用RPM package方式手动安装了。

## 操作步骤

### 检查系统信息

```sh
[root@VM_0_3_centos ~]# lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.2.1511 (Core) 
Release:        7.2.1511
Codename:       Core
[root@VM_0_3_centos ~]# 
```

### 卸载旧版本

```sh
[root@VM_0_3_centos ~]# sudo yum remove docker
Loaded plugins: fastestmirror, langpacks
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
No Match for argument: docker
No Packages marked for removal
[root@VM_0_3_centos ~]# sudo yum remove docker-client\
> docker-client-latest\
> docker-common\
> docker-latest\
> docker-latest-logrotate\
> docker-selinux\
> docker-engine-selinux\
> docker-engine
Loaded plugins: fastestmirror, langpacks
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
No Match for argument: docker-clientdocker-client-latestdocker-commondocker-latestdocker-latest-logrotatedocker-selinuxdocker-engine-selinuxdocker-engine
No Packages marked for removal
[root@VM_0_3_centos ~]# 
```
之前没有安装过，所以卸载旧版本这步可以忽略。

### 安装必要的包

```sh
[root@VM_0_3_centos ~]# sudo yum install -y yum-utils \
> device-mapper-persistent-data \
> lvm2
```

当输出以下信息，则说明安装成功

```sh
Updated:
  device-mapper-persistent-data.x86_64 0:0.7.0-0.1.rc6.el7_4.1    lvm2.x86_64 7:2.02.171-8.el7    yum-utils.noarch 0:1.1.31-42.el7   

Dependency Updated:
  device-mapper.x86_64 7:1.02.140-8.el7                                device-mapper-event.x86_64 7:1.02.140-8.el7                    
  device-mapper-event-libs.x86_64 7:1.02.140-8.el7                     device-mapper-libs.x86_64 7:1.02.140-8.el7                     
  lvm2-libs.x86_64 7:2.02.171-8.el7                                    python-urlgrabber.noarch 0:3.10-8.el7                          
  rpm.x86_64 0:4.11.3-25.el7                                           rpm-build-libs.x86_64 0:4.11.3-25.el7                          
  rpm-libs.x86_64 0:4.11.3-25.el7                                      rpm-python.x86_64 0:4.11.3-25.el7                              
  yum.noarch 0:3.4.3-154.el7.centos.1                                 

Complete!
```

### 配置仓库

```sh
[root@VM_0_3_centos ~]# sudo yum-config-manager \
> --add-repo \
> https://download.docker.com/linux/centos/docker-ce.repo
Loaded plugins: fastestmirror, langpacks
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
```

### 安装Docker

```sh
[root@VM_0_3_centos ~]# sudo yum install docker-ce
```
安装期间会有两次提示，第一次提示是告知下载内容大小，询问是否下载，第二次是提示GPG密钥验证，这两次提示都选则yes。  
当输出以下信息，标识安装成功

```sh
Installed:
  docker-ce.x86_64 0:18.03.0.ce-1.el7.centos                                                                                           

Dependency Installed:
  audit-libs-python.x86_64 0:2.7.6-3.el7 checkpolicy.x86_64 0:2.5-4.el7               container-selinux.noarch 2:2.42-1.gitad8f0f7.el7
  libcgroup.x86_64 0:0.41-13.el7         libseccomp.x86_64 0:2.3.1-3.el7              libsemanage-python.x86_64 0:2.5-8.el7           
  pigz.x86_64 0:2.3.4-1.el7              policycoreutils-python.x86_64 0:2.5-17.1.el7 python-IPy.noarch 0:0.75-6.el7                  
  setools-libs.x86_64 0:3.3.8-1.1.el7   

Dependency Updated:
  audit.x86_64 0:2.7.6-3.el7  audit-libs.x86_64 0:2.7.6-3.el7  libsemanage.x86_64 0:2.5-8.el7  policycoreutils.x86_64 0:2.5-17.1.el7 

Complete!
```

此时Docker已经安装了，但是还未启动，并且会建立一个用户组:docker，但是没有这个组里现在没有任何用户，可以使用以下命令查看

```sh
[root@VM_0_3_centos ~]# cat /etc/group | grep docker
docker:x:988:
[root@VM_0_3_centos ~]# 
```

可以查看Docker版本

```sh
[root@VM_0_3_centos ~]# docker -v
Docker version 18.03.0-ce, build 0520e24
[root@VM_0_3_centos ~]# 
```

启动Docker

```sh
[root@VM_0_3_centos ~]# sudo systemctl start docker
[root@VM_0_3_centos ~]# 
```

验证Docker是否安装正确，下面这条命令会下载一个hello-world的镜像，并且启动它,它会打印出信息，然后退出。

```sh
[root@VM_0_3_centos ~]# sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete 
Digest: sha256:97ce6fa4b6cdc0790cda65fe7290b74cfebd9fa0c9b8c38e979330d547d22ce1
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

[root@VM_0_3_centos ~]# 
```

## 小结

本篇主要的内容就是介绍在有网络CentOS环境下Docker CE的下载和安装，下载前要检查自己系统的版本必须在CentOS 7。如果之前安装过Docker，则需要先删除系统中存在的旧版本。
安装过程主要分为三部分，第一部分是依赖包的下载和安装，第二部分是配置Docker仓库，第三部分就是Docker的下载和安装，非常简单。最后我们测试了一下是否正确安装成功。强烈建议的是如果在有网络的环境下，尽量采用本篇的方式来安装，这样对以后版本的升级和管理都非常方便，那实在是没有网络的话，就只能采用RPM包手动安装了。  
本篇已经成功安装了Docker CE，在接下来的章节中，将要介绍Docker使用的基础知识。