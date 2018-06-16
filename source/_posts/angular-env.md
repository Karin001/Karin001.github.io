---
title: angular-windows环境搭建
date: 2018-02-16 15:19:36
tags: angular js typescript fullstack
intro: windows平台的环境搭建是痛苦的，尤其是国内这样的环境...
---


### **1. 先装nvm**
- 去[这里](https://github.com/coreybutler/nvm-windows/releases) 下载。
- 在硬盘建好目录结构，比如f:/dev/nvm,将第一步下载下来的东西解压到此目录，然后运行install，选择setting文件输出目录。
- 找到生成的setting文件，将其剪切到f:/dev/nvm目录下，打开,更改如下
```
root:  f:/dev/nvm
path: f:/dev/nodejs 
arch: 64 
proxy: none
node_mirror: http://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
- 去系统环境变量，添加系统变量NVM_HOME，将其设置为 f:/dev/nvm ，添加系统变量NVM_SYMLINK，将其设置为 f:/dev/nodejs ，在path里面添加%NVM_HOME%;%NVM_SYMLINK%，放在最后面。f:/dev/nodejs目录里面放的是nodejs，它会根据你在nvm里面选择的node版本进行动态选择。
- 在命令行中试试 nvm -v 看好不好使
- 在nvm中安装nodejs，安装时会自动安装npm ，切到命令行输入
```
nvm install [node版本]
```
- 在nvm中选择node版本，切到命令行输入
```
nvm use [node版本]
```
- 看看f:/dev/nodejs有没有node
---

### **2. 装npm**
- 在f:/dev/nodejs文件夹下打开命令行，npm config set prefix "f:/dev/nvm/npm"， 设置npm的全局路径，接着 npm install -g npm, 安装一个全局的npm。
- 接着在环境变量里面建立NPM_HOME，将其设置为f:/dev/nvm/npm，在path里面添加%NPM_HOME%，放在最前面，要在%NVM_HOME%;%NVM_SYMLINK%之前。
- 接着打开命令行，试试npm -v好不好使
- 设置一下镜像 npm config set registry https://registry.npm.taobao.org 
- 装一个cnpm npm install cnpm -g
- 到‘用户’目录下，找到.npmrc文件，用记事本打开，在prefix=f:\dev\nvm\npm这一行下面添加
```
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
electron_mirror=https://npm.taobao.org/mirrors/electron/
registry=https://registry.npm.taobao.org
```
- 上一步可以防止以后用npm装东西时node-sass等文件装不上的问题
---

### **3. 装windows-build-tools**
- 装这个是为了防止以后一些麻烦，我也不知道为什么，反正解决了我的问题\(^o^)/~
- 打开命令行
```
npm install --global --production windows-build-tools


```
> 如果安装过程中报SYSTEM32之类执行cmd失败的故障，很可能是环境变量里面没有加"C:\Windows\System32\WindowsPowerShell\v1.0"
- 装完之后 
```
npm config set msvs_version 2015 --global
npm install -g node-gyp
```
- 装visual studio 2015 express
- 关闭命令行
----

### **4. 装angular/cli**
- 先装这个
```
npm install -g typescript typings 
```
- 再升级下这个
```
npm update -g gcc
```
- 装angular/cli
```
npm install -g @angular/cli
```
- 试下好不好用
```
ng new test --skip-install --style=scss
```

> 以上会跳过npm包安装，我们可以手动调用cnpm进行安装

---

### **5. 装git**
- 去[这里](https://git-scm.com/download)下载
- 安装，一路点击
- 安装完后打开git bash，敲入以下内容，替换成自己的名字和邮箱
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
- 创建一个ssh钥匙
```
ssh-keygen -t rsa -C "youremail@example.com"
``` 
- 创建完成后应该有两个文件，找到‘用户’目录下的.ssh文件夹下的id_rsa.pub，用记事本打开，复制里面的内容。
- 打开github，登录你自己的账号，找到setting，进去后找到ssh那一栏，点击new ssh key，将复制的内容粘贴进去，大功告成。