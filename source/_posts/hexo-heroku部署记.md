---
title: Hexo-Heroku部署记
tags: []
id: '58'
categories:
  - - 开发/服务器踩坑记录
date: 2022-02-06 02:59:55
---

这篇文章是在刚开始部署的时候创建的，现在一看实在是能踩的坑全部都踩过一遍了（

心血来潮想在新注册的Heroku上部署点什么东西，那就是Hexo了，其实是想给一起玩的朋友建一个发发疯的主页。

这次的部署过程主要基于[Hexo官方文档](https://hexo.io/zh-cn/docs/)以及[Hexo - Heroku 部署教學](https://blog.kennycoder.io/2019/08/04/Hexo-Heroku%E9%83%A8%E7%BD%B2%E6%95%99%E5%AD%B8/)，在windows平台上进行本地操作，由于之前安装过了Git和Node.js（之后发现单独的某个node版本还是有些局限，见后文），这部分的安装过程略过。

首先根据教程安装Heroku CLI并创建app，这一步并没有太大问题。在运行`heroku login`时出现了第一个问题——国内不使用代理无法连接登录，会出现Invalid Request报错，但使用代理后又会出现IP不匹配的问题。在这一步完成之前是无法使用git推送到heroku上的，所幸在[解决 Heroku CLI 登录问题](https://blog.csdn.net/qq_42951560/article/details/109717160)中找到了方案，使用`heroku login -i`启用密码登录绕过网页登录。

然后就是hexo和部署工具的安装，命令如下：

```
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
$ npm install hexo-deployer-heroku --save
```

这几步完成后，修改博客目录下的`_config.yml`，在deploy下添加如下配置：

```
deploy:
  type: heroku
  repo: <repository url>
```

并在url一栏中修改网址，接着理论上运行

```
$ hexo clean
$ hexo g
$ hexo d
```

就可以访问了，但由于是在heroku网页上创建的app，又是直接下载hexo的文件，这时候博客文件夹是没有git仓库的属性的，会报错

```
 fatal: Not a git repository (or any of the parent directories): .git 
```

需要在目录下`git init`一下。

在这之后的`hexo d`中，又出现了新的问题。

```
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
```

最后在一个issue中找到了问题的原因是hexo不支持14以上版本的node，现在大版本都16了你怎么毛病这么多，于是试图下载13.9.0，索性用nvm做版本管理。

在官网下载nvm后安装，记得安装到需要管理员权限的文件夹时（比如Program Files）要用管理员权限运行终端。在nvm安装目录的settings.txt中添加如下两条镜像。

```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

然后`nvm install 13.9.0`，报错npm无法下载，按照网上方法全试了一遍之后发现

[![](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-2-1024x703.png)](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-2.png)

岂止是更新不及时，整个淘宝的npm镜像都寄了，绝绝子

[![](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-3.png)](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-3.png)

遂删除npm镜像源，一次安装成功，在use 13.9.0时报错

[![](https://blog.inscripoem.com/wp-content/uploads/2022/02/image.png)](https://blog.inscripoem.com/wp-content/uploads/2022/02/image.png)

记得之前说的吗？使用管理员权限打开终端即可

然后就是激动人心的重装npm包，`hexo clean`之后`hexo g`，`hexo d`，喜闻乐见的出了新错误

```
npm ERR! missing script: start
```

之后发现重新删除app再创建一个就好了，应该是残留文件的问题

[![](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-5-1024x572.png)](https://blog.inscripoem.com/wp-content/uploads/2022/02/image-5.png)

锵锵——再之后就是回归hexo本身的折腾了，这次算是对heroku的机制也有了一个简单的了解，之后找个机会嫖个每月450小时的运行时长吧（