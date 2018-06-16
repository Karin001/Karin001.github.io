---
title: hexo-极速建bolg-windows
date: 2018-06-16 15:06:34
tags: hexo blog github
intro: 用hexo快速搭建静态博客
---
> 这个博客就是用hexo搭建的静态博客，很快就可以搞定，有多种主题可以选择
> hexo是静态博客，不需要后台，不需要数据库，唯一需要掏钱的地方就是购买域名（如果你需要一个与众不同的域名，比如我这样的，那么你需要支付几十元）
> 静态博客的意思就是你大概只能用来写写文章，发发图片。其他的功能则要用一些工具来实现，比如评论。

搭建流程大概是这个样子：  
1. 准备好hexo博客所需的一系列环境（这是最繁琐的）
2. 在本地快速搭建博客（秒开）
3. 将博客推上线（秒开）
4. 改域名（如果你需要一个自己的域名，那么这一步就是必须的。这一步会涉及到购买域名，以及一些dns配置）


搭建好后写博文也非常方便，大概是这个样子使用：
1. 打开你的hexo博客文件夹（用你最喜欢的文本编辑器，我用的vscode）
2. 新建博文：在命令行中输入 hexo new [文件名字]
3. 编辑刚刚生成的md文件，保存
4. 输入hexo g 重新生产静态文件
5. 输入hexo s 在localhost:4000查看修改后的博客（这一步不是必须）
6. 确认无误后 输入hexo d 更新线上博客
---

####  hexo环境搭建

##### git
1. 去[这里](https://git-scm.com/download)下载
2. 安装，一路点击
3. 安装完后你的右下角开始菜单里应该能找到这两个东西
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/88133562.jpg)
4. 打开git bash，敲入以下内容，替换成自己的名字和邮箱
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

5. 创建一个ssh钥匙
```
ssh-keygen -t rsa -C "youremail@example.com"
```
6. 创建完成后应该有两个文件，找到‘用户’目录下的.ssh文件夹下的id_rsa.pub，用记事本打开，复制里面的内容。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/57278515.jpg)
7. 打开github，登录你自己的账号，如果没有则注册一个。在页面中找到setting
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/40893030.jpg)
8. 进去后找到ssh那一栏，点击new ssh key，将复制的内容粘贴进去，大功告成。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/33837002.jpg)

##### node
1. 新建一个文件夹，我这里是f盘（随意），建一个叫npm的文件夹（f:/npm），
2. 去[这里](https://nodejs.org/en/)下载LTS版本的node
3. 安装，一路点击，注意装的位置
4. 装完之后去安装路径，找到npm.cmd这个东西
5. 打开命令行（按住shift敲鼠标右键，选择在此处打开命令行或者PowerShell），输入`npm -v`， 接着输入`node -v`，会显示版本号，表示安装OK。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/22924758.jpg)
6. 接着输入`npm config set prefix "f:/npm"`， 设置npm的全局路径，接着输入 `npm install -g npm`, 安装一个全局的npm。
7. 接着在环境变量里面（右键点击电脑->属性->高级系统设置->环境变量->用户变量）建立NPM_HOME，将其设置为f:/npm，在path里面添加%NPM_HOME%。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/20474890.jpg)
8. 接着关掉所有打开的文件夹和命令行，回到桌面，重新打开一个命令行，试试npm -v好不好使
9. 设置一下镜像，输入： `npm config set registry https://registry.npm.taobao.org` 
10. 装一个cnpm，输入： `npm install cnpm -g`
11. 到‘用户’目录下，找到.npmrc文件，用记事本打开，在prefix=f:\npm这一行下面添加
```
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
electron_mirror=https://npm.taobao.org/mirrors/electron/
registry=https://registry.npm.taobao.org
```
##### hexo
1. 打开命令行，输入 `npm install -g hexo-cli` ，安装完成后输入`hexo -v`查看是否安装成功
---

#### 在本地搭建hexo博客

#### 初始化博客
1. 选择一个合适的路径，新建一个文件夹，该文件夹即将成为你的博客文件夹。在新建的文件夹下打开终端，输入`hexo init`，等待安装完成，完成后接着输入`npm i`,安装必要的组件，完成后接着输入`hexo g`,生成博客静态文件，接着输入`hexo s`,打开浏览器，在地址栏中输入`localhost:4000`回车之后就能看见博客了。
2. 在终端中按住Ctrl+C键可以退出。

#### 更换主题
在[这里](https://hexo.io/themes/)能找到很多主题
1. 点击图像可以预览，我们预览之后可以点击下面的文字去github克隆这个主题，这里假设我们选了下面框住的这个，当然你可以随意选。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/54631383.jpg)
2. 到github之后，就可以按照他的说明文档尽心主题安装了，注意看安装说明，如果有特殊的步骤都会注明。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/52365917.jpg)
3. 复制`git clone https://github.com/huyingjie/hexo-theme-A-RSnippet.git themes/a-rsnippet`这一句
4. 到你的博客文件夹里打开终端，将上面那句粘贴进来，回车。
5. 可以看到博客文件夹下的theme文件夹里多了一个主题
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/52365917.jpg)

