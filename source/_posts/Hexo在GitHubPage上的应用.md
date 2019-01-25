---
title: Hexo在GitHubPages上的应用
date: 2019-01-16 15:36:12
tags: GitHub,Hexo,博客
index_img: http://cdn.pfish.xyz/pic/20190116/3hyL7kWcVKrO.jpg?imageslim
---
在GitHub上用Hexo主题搭建一个简单的个人博客，使用MarkDown格式发布。

<!--more-->
### GitHubPages

>当你在 GitHub 新建一个名为 yourusername.io 的 repositories 时，就默认创建了一个网址为  https://yourusername.github.io 的GitHubPage

你可以在上面放上一个 index.md 或者 index.html 之类的**静态页面**, 这样别人就能通过域名访问了！也可以绑定自定义域名，然后在你的域名控制台添加一条 CNAME 记录来实现自定义域名访问。我的: https://pfish.xyz

你可以用此来搭建一个免费的个人博客。在我看来，这也比在云服务器部署一个 wordpress 之类的个人网站其实要方便一些，也不用过多的维护。

### Hexo  

> Hexo是一个博客框架，支持Markdown，只需一条指令即可部署到 GitHub Pages, Heroku 或其他网站。

[Hexo官网](https://hexo.io/)

你需要知道一些 Git 的基本使用，并且在电脑上安装 Node.js 和 Git，
安装及部署可参考其他写的更好的文章。（我很懒）

 **不过别急，建议看完本篇再动手干**

### 跳坑及操作指南
>类似教程都相当多了，个人建议主要参考官方文档，下面列出一些自己踩过的坑，以及一些奇技淫巧，相信很多问题大部分人也会遇到，回头看也是相当的脑残。

#### 用不同分支保存静态网站文件和博客源文件
（配置好的情况下）在项目文件夹中使用命令 `hexo d` 会自动将相关文件push 到你仓库的 master 分支里。在你的库里，master 只负责保存你部署的静态网站的相关文件：
 - index.html等网页文件
 - js,css等和上传的图片img文件
 - CNAME 域名信息文件（在 source 文件夹新建一个）


<img src="http://cdn.pfish.xyz/pic/20190116/BbiHqxQLLkHu.png?imageslim" width="100%"/>


 所以！千万不要将项目里所有的文件手动 push 到 master 分支。
 
  **而如果想把源文件同时备份一份到GitHub上呢？**

  那么就在你的库中新建一个分支，并设为主分支。（这不影响网站的访问和显示）

  然后将它 clone 到本地某个位置，将项目文件夹中除了 .git 文件夹之外的所有文件和文件夹都 copy 过去，Themes主题文件夹中的 .git 文件夹删除才能commit，然后可以用 GitBash 检查一下是否在你新建的那个分支，接着就可以把项目文件备份到 GitHub，同时使用 `hexo d` 也会自动的将你写好的 md 文件 push 到master 分支。非常爽！

#### npm 和 Git

npm这东西接触的不太久，用起来有时候一言难尽，建议修改一下源，install 的时候坐和放宽，很多时候的问题就是 npm 没装好 或者是git clone 项目的时候出的问题，多试几次，失败了就全删了再试几次，疯了就好了。

#### Themes主题

尝试过一些主题，`hexo g` 和 `hexo s` 后本地运行可能会出现  index.html 丢失的情况，部署上去一样。我的解决办法就是换一个就好了。

主题相关配置文件在该主题文件夹下的 _config.yml 文件中，你可以按需更改成你满意的样式。
