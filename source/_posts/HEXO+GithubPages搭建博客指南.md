---
title: hexo+GithubPages博客搭建指南
date: 2017-02-08 13:52:05
tags:HEXO
---
> Hexo是一个快速、简介且高效的博客框架，配置了多种主题，定制自己风格的博客。PS 我的博客地址传送门在这：[shelia's blog][1]

[TOC]

#前期准备

##安装Git Bash
 
github是程序员的标配，因为使用的是Windows系统，所以下了个github的客户端。里面包含了Git Bash和图形界面的Git GUI,使用哪个看个人习惯。建议使用GitBash，可以在日常的操作中熟悉常用指令。

廖雪峰的Git教程有提供国内的镜像下载，或者可以到这个地址下载https://desktop.github.com/。
 
##安装Node.js

Nodejs我是直接下载的客户端，直接用搜索引擎搜索一下就能找到。
**注意：**安装时一路next的时候请选择Add to Path方式安装。

##安装hexo

所有必须的软件安装后，打开终端（随你选择）输入以下命令安装hexo：

> npm install -g hexo-cli

**注意：**安装的时候如果出现warming不要管它之后也可以用，如果是ERROR的话可能需要你用管理员权限运行终端来安装。

为了测试hexo是否正常安装，我们可以通过以下命令来测试（我的所有命令都是在管理员权限下的）。

在你喜欢的任意一个地方，新建一个博客文件夹：

> hexo init hexoblog

安装依赖：

> npm install

本地运行测试

> hexo s

如果成功会打印:Hexo is running at  http://localhost:4000/. Press Ctrl+C to stop,再打开你的浏览器输入localhost:4000地址就可以看到你的博客了。

#在github上创建GithubPages

##配置SSH

github本地和远程连接有两种方式，一种是通过https，另一种是SSH，推荐后一种。

配置SSH的步骤如下：

- 在本地生成公钥和私钥,命令如下：

 > ssh-keygen

- 找到你保存公钥和私钥的文件夹，用记事本打开之前生成的id_rsa.pub文件,复制里面的公钥字符串。打开你的github网站，在setting里面的SSH keys，再点击Add SSH key 按钮添加，title域随便命名，Key域粘贴之前复制的内容。

##创建仓库

github有专门的一种格式来创建博客仓库的。新建一个仓库，仓库名为:**youusername.github.io**.

#建站

##克隆远程仓库

为了管理方便，建议再一个分支上存放hexo博客的源代码，master分支用来发布静态博客，步骤如下：

 - 在你的仓库中新建一个分支，我命名为hexo，然后切换到仓库的settitng的branch选项设置为默认分支。
 - 打开Git Bash，复制远程仓库
    > git clone git@github.com:username/username.github.io.git
 - 克隆好远程仓库我们就可以开始建站了。在执行其它命令前，把根目录下的.git文件夹复制到另一个地方保存先，因为 hexo init命令会覆盖掉它。
 
##搭建hexo博客

打开终端，依次执行以下命令：

> npm install hexo
> hexo init
> hexo install

复制之前保存在另一个地方的.git文件夹再放到根目录下。

> npm install hexo-deployer-git

**注意：**这些命令都是工作在hexo分支上，以后也是。

修改_config.yml中的deploy参数，分支应为master，可以按照如下设置：

> deploy:
>     type:git
>     repo:（复制你的项目地址到这里）
>     branch:master
 
依次执行 `git add .`,`git commit -m "..."`,`git push origin hexo`提交网站相关的文件
 
执行`hexo g -d`生成网站并部署到GitHub上。

此方法由知乎上的一位网友提供https://www.zhihu.com/question/21193762，但是必须记得复制.git文件夹不然本地仓库就失效了。

#配置

在根目录下有一个`_confug.yml`文件夹，在里面你可以通过相关字段来设置自己的个人信息。

##配置主题

我的博客使用的主题是nexT，网上有具体的使用文档 [NexT使用文档传送门][2]

同时在文档中也介绍了如何安装第三方插件。

#添加域名

添加域名有三个步骤：

- 首先现在万网上买一个域名
- 下一步注册DNSpod，然后添加域名，添加记录即可
- 返回在你的仓库setting里面绑定域名，它会生成一个CNAME文件。

**注意：**我在绑定域名后在本地重新提交代码部署静态博客时，CNAME文件总是被覆盖。解决办法是下载CNAME文件，放到本地站点目录下的source文件夹，注意CNAME是无格式的，不然会被部署到网站上。



   


  [1]: http://shelia.site/
  [2]: http://theme-next.iissnan.com/%20%E2%80%9CNexT%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3%E4%BC%A0%E9%80%81%E9%97%A8%E2%80%9D