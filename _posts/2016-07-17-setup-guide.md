---
layout: post
title: 用Jekyll模板搭建Github页面-Windows
description: "适用于Windows"
tags: [网页搭建, 网络编程]
categories: [Tech]
---
### 安装基本软件

- 首先安装一个能在windows环境下运行的包管理器[**Chocolatey**](https://chocolatey.org/)

- 因为Jekyll是用Ruby写的，所以要安装Ruby，在控制台中输入**choco install ruby -y**回车

- 关闭控制台，然后再打开控制台并输入**gem install jekyll**，这样Jekyll就装好了

- 重新打开控制台，输入**chcp 65001**避免编码问题

- 安装Ruby开发环境，在控制台中输入：

  **choco install ruby -version 2.2.4**

  **choco install ruby2.devkit**

- 在**C:\tools\DevKit2**文件夹中打开控制台，执行命令 **ruby dk.rb init**，产生config.yml文件


<!-- more -->
- 编辑config.yml文件，并加入

  \-  **C:/tools/ruby23**

- 然后执行**ruby dk.rb install**

- 安装Nokogiri gem，在控制台中依次执行以下命令：

  **cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libxml2** 回车

  **cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libxslt** 回车

  **cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libiconv** 回车

  gem install nokogiri --^
     --with-xml2-include=C:\Chocolatey\lib\libxml2.2.7.8.7\build\native\include^
     --with-xml2-lib=C:\Chocolatey\lib\libxml2.redist.2.7.8.7\build\native\bin\v110\x64\Release\dynamic\cdecl^
     --with-iconv-include=C:\Chocolatey\lib\libiconv.1.14.0.11\build\native\include^
     --with-iconv-lib=C:\Chocolatey\lib\libiconv.redist.1.14.0.11\build\native\bin\v110\x64\Release\dynamic\cdecl^
     --with-xslt-include=C:\Chocolatey\lib\libxslt.1.1.28.0\build\native\include^
     --with-xslt-lib=C:\Chocolatey\lib\libxslt.redist.1.1.28.0\build\native\bin\v110\x64\Release\dynamic 回车


### 新建Github页面并安装Jekyll模板

- 我的模板来自[**HPSTR**](https://github.com/mmistakes/hpstr-jekyll-theme)，从这里把模板fork到自己的Github

- 建立自己的Github主页，在自己的Github中建立一个新的depository，命名为username.github.io，**这里的username必须是自己的github用户名**，比如我的用户名是einsteinliu，就必须新建一个叫einsteinliu.github.io的Depository。

- 将新建的github页面的Depository克隆到本地文件夹，比如D:\Blog：

  **git clone https://github.com/*username*/*username*.github.io**

  然后会出现D:\Blog\username.github.io这个文件夹，这就是本地Depository

- 将之前Fork到的[**HPSTR**](https://github.com/mmistakes/hpstr-jekyll-theme)模板下载下来，复制到github页面的本地文件夹D:\Blog\username.github.io

- 在这个文件夹中打开控制台，依次执行以下命令：

  **git add --all回车**

  **git commit -m "init"回车**

  **git push -u origin master回车**

  **输入Github的用户名密码回车，等待代码上传完成**

- 从浏览器访问einsteinliu.github.io，新页面应该就可以访问了


以后每次改动页面后，在页面代码所在文件夹中执行

**bundle exec jekyll serve**

页面就会重新编译

  ​

  ​

  ​

  ​

