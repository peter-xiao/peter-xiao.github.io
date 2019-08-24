---
title: 如何搭建hexo博客
date: 2019-08-6 02:51:37
tags: 
  - 搭建
  - 博客
categories:
---
博主为mac pro，首先打开终端，进入自己用户名的目录下，获取root权限`sudo su`
<!--more-->
### 安装环境

*   首先下载node.js，`brew install node`
*   再利用cnpm全局下载hexo，因为墙的问题，用npm下载hexo很慢，  
    `npm install -g cnpm --registry=https://registry.npm.taobao.org`
*   利用cnpm全局下载hexo，`cnpm install -g hexo-cli`

### 初始化博客

*   在自己用户名的目录下创建一个新的文件夹blog，这既是hexo的要求，也是良好编程的要求，`mkdir blog`
*   进入blog，`cd blog`
*   初始化博客， `hexo init`
*  *如果下载很慢，可以用这种方法来获得加速。另外打开一个终端，执行`sudo vim /etc/hosts`，对hosts进行编辑，在文件结尾加上如下代码* 
```
192.30.253.112 github.com   
151.101.185.194 github.global.ssl.fastly.net  
```

*前往[查询](http://tool.chinaz.com/dns?type=1&host=github.com&ip=)，获得github.com和github.global.ssl.fastly.net的ip，ttl值理论上来说越小ip网速越快，代码中的ip并非是最好的，可以多次尝试，ip会几个月变动一次，注意及时更新*

### 部署博客

*   登入github，注册一个账号，需要翻墙，使用不需要翻墙
    
*   创建一个自己的本地仓库repository，名字必须为`[github_name].github.io`
    
*   回到第一个终端，安装仓库和宿主机链接的工具 `cnpm install --save hexo-deployer-git`
    
*   进行配置，`vim _config.yml`，移动到`Deploment`
```
deploy:   
  type: git   
  repo: https://github.com/[github_name]/[github_name].github.io.git   
  branch: master  
```
*   部署到远端，`hexo d`，输入github账号，密码，第一次还要创建钥匙串
    

# 浏览器输入[github_name].github.io就可以访问了
