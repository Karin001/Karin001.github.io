---
title: hexo-极速建bolg(for windows)
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

###  hexo环境搭建

#### git
1. 去[here](https://git-scm.com/download)下载
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

#### node
1. 新建一个文件夹，我这里是f盘（随意），建一个叫npm的文件夹（f:/npm），
2. 去[here](https://nodejs.org/en/)下载LTS版本的node
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
#### hexo
1. 打开命令行，输入 `npm install -g hexo-cli` ，安装完成后输入`hexo -v`查看是否安装成功
---

### 在本地搭建hexo博客

#### 初始化博客
1. 选择一个合适的路径，新建一个文件夹，该文件夹即将成为你的博客文件夹。在新建的文件夹下打开终端，输入`hexo init`，等待安装完成，完成后接着输入`npm i`,安装必要的组件，完成后接着输入`hexo g`,生成博客静态文件，接着输入`hexo s`,打开浏览器，在地址栏中输入`localhost:4000`回车之后就能看见博客了。
2. 在终端中按住Ctrl+C键可以退出。
#### 更换主题
在[这里](https://hexo.io/themes/)能找到很多主题
1. 点击图像可以预览，点击文字可以去github克隆这个主题。这里我们选了这个next主题，外观简洁，功能多样，推荐的人很多。当然你可以随意选。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/4302047.jpg)
2. 到github之后，就可以按照他的说明文档尽心主题安装了，注意看安装说明，如果有特殊的步骤都会注明。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/42194811.jpg)
3. 复制`git clone https://github.com/theme-next/hexo-theme-next themes/next`这一句
4. 到你的博客文件夹根目录下，打开终端，将上面那句粘贴进来，回车。
5. 可以看到博客文件夹下的theme文件夹里多了一个主题
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/73405017.jpg)
6. 点击上图中的_config.yml文件，按照下图修改
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/86230016.jpg)
7. 在终端中清一下静态文件`hexo clean`，接着重新生成`hexo g`，本地serve一下`hexo s`，在loacalhost:4000能看到主题已经更换了
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/60554822.jpg)
8. 现在试着发一篇文章，在终端中输入`hexo new [文章名字]`,在新增的[文章名字].md文件中输入一些文字，保存，我这里叫first.md。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/20438466.jpg)
9. 在终端中输入`hexo g`，接着`hexo s`，以后每次增加新文章都要这么做。现在刷新浏览器，可以看到文章已经更新了。
8. 等你熟练了一些之后，就可以去网上搜索下next这款主题的配置方法，自行尝试该主题的丰富功能。
---

###  本地博客上线
我们利用github page将本地的博客推上线
1. 登录我们的github账号，新建一个仓库
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/87092925.jpg)
2. Repository name仓一栏输入`[你的github用户名].github.io`，类似下面这个样子，注意替换用户名。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/16665113.jpg)
3. 创建好之后点击进入你刚才建的仓库，找到settings，点进去
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/8047537.jpg)
4. 一直往下找，找到github page这一栏，上面有你的page地址，复制之后，点击下图的按钮
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/26775707.jpg)
5. 访问你的github page，成功后回到hexo博客文件夹，打开根目录下的_config.yml文件，按图下配置，repository写你自己的。
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/50686748.jpg)
6. 安装一个部署插件，在终端中输入`npm install hexo-deployer-git --save`
7. 装好之后开始部署，输入`hexo d`，部署完成之后githubpage就变成了你hexo博客了。
---

### 购买域名
我用的阿里云完成的域名购买，比较方便，因为域名要实名制备案，阿里云这块就全部集成了，按照他的流程来操作即可，可以随意。

---

### 域名解析
域名需要通过dns解析之后才能被识别访问。
1. 如果你的域名是顶级域名，像这个博客这样的一个单词，就去[here](https://help.github.com/articles/setting-up-an-apex-domain/),查阅步骤。如果不是，就去[here](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)找到帮助。
2. 打开域名控制台添加一条a记录，除了记录值一栏，其余按照下图填写
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/95437121.jpg)
3. 记录值按照第一步打开的页面中如下部分填写，你的应该不一样，填写其中一条地址
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/312175.jpg)
4. 可以重复第二，第三步，将所有的记录值都填写一遍，完成后将填写的几条配置全部启用。
5. 在github里设置page的自定义域名
![](http://yu-mdfile.oss-cn-hangzhou.aliyuncs.com/18-6-16/11204190.jpg)
6. 等几分钟，大功告成