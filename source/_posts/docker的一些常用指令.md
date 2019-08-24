---
title: docker的一些常用指令
date: 2019-07-30 02:17:41
tags: 
 - docker
 - 笔记
categories: 实习
---
相关的东西网上都有，这里主要分入门和进阶。
<!--more-->
### 入门

`Docker`打包应用程序及该程序的依赖，放在`image`文件里。

##### 以下命令皆在容器外执行：

*   抓取`iamge`文件，`docker image pull [image_name]`
*   定义一个容器，`docker run --name [docker_name] [image_name]`（事实上输入这个命令，就会自动执行上方命令）
*   停止容器，`docker stop [docker_name]`
*   启动一个已停止的容器，`docker start [docekr_name]`
*   删除容器，`docker kill [docker_name]`
*   查看运行中的容器，`docker ps -a(包括已停止的容器)`
*   进入容器，`docker exec -it [docker_name] /bin/zsh(bash)`
*   将容器中的文件拷贝到宿主机上，`docker cp [local_position] [docker_position]`
    
##### 以下命令皆在容器内执行：
    
*   退出容器，`ctrl+d`

### [](#进阶 "进阶")进阶

定义一个容器时，有各种各样的参数需要添加，对于不同的容器，需要配置的参数也不一样，详细了解各种参数，才能配置出符合要求的容器。更多信息通过`docker --help`了解。

*   守护进程，`-d`
*   将容器和宿主机的端口随机相连接，`-P`
*   将容器和宿主机的端口定向连接，随机的能保证宿主机的端口是有效的，`-p`
*   添加版本信息，`-v`
*   将宿主机的某个文件夹的信息连接到容器的特定文件夹，便于容器内信息的管理，`[localhost_position]:[docker_position]`

创建一个镜像，然后推到dockHub，这个博主还不会，等以后更新。
