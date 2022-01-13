# 个人博客快速搭建教程

> 作为一名 `IT` 技术控，应该要知道 `好记性不如烂笔头` 这句话的意义，特别是在技术快速更新迭代的时代，我们更要有持续学习的能力。虽然我们不是纯粹的极客，但是我们依然需要经常总结、记录，以便后续持续复盘，而现在通用的博客网站，例如博客园、`CSDN` 、掘金、思否、知乎等，都不能让你随心所欲的修改，特别是样式问题，所以今天我们来讲解一下如何快速搭建个人博客，并且使用程序员最爱的 `markdown` 语法进行快速的博客编写 :rocket:

### 效果展示

开始之前，我们先来看一下博客的展示效果

#### 主页

> 主页可以自定义背景色，框架拐角处挂在个人 `GitHub` 角标

![common_blog_home](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/common_blog_home.png)

#### 全站检索

> 支持全站检索功能。通过输入关键字，就能够搜索出历史相关的博客记录

![common_blog_search](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/common_blog_search.png)

#### 章节跳转

> 支持章节跳转功能

![common_blog_next](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/common_blog_next.png)

#### 菜单收起

> 支持菜单收取功能

![common_blog_menu](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/common_blog_menu.png)

#### 响应式布局

> 支持自适应、响应式布局。能够自动适配 `PC` 端和移动端

![common_blog_flex](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/common_blog_flex.png)

### 快速开始

#### 项目框架下载

首先，在任意位置执行右键打开 `cmd` 或者`powershell` 或者  `bash` 然后执行如下语句

```powershell
git clone git@github.com:JeremyWu917/jeremy-blog.git
```

然后，在 `jeremy-blog` 文件夹下右键用 `VSCode` 打开，可以看到整个项目的结构如下：

![image-20220113160746830](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220113160746830.png)

#### 项目目录介绍

- `_image` 存放资源文件，这里主要放的是 `logo` 文件
- `_media` 存放媒体样式文件，这里主要放的是整个博客站的样式文件
- `.vscode` 开发工具 `VSCode` 的配置信息，这里主要是本地 `liveServer` 端口信息
- `zh-cn` 博客列表文件，后续新写的博客文件放在这里即可
- `_coverpage.md` 主界面信息
- `_navbar.md` 导航信息
- `_sidebar.md` 左侧菜单信息，后续每次新增博客文件后都需要在此处添加一下路径信息
- `CNAME` 域名信息，这里主要是配合部署到私有域名使用的文件
- `index.html` 整个博客网站的起始文件
- `LICENSE` 项目协议信息，这里使用的是非常宽松的 `MIT` 协议
- `README.md` 项目简介信息，也是后续博客首页的信息

###  定制化修改

- `logo` 修改 `_image` 
- 主界面信息按需进行修改 `_coverpage.md` 
- 博客网站的起始文件修改 `index.html` 
- 域名信息修改（这里如果不启用域名，需要删除此文件） `CNAME`
- 项目简介信息 `README.md` 

> **注意**
>
> 后续每次写完博客文件后，都需要将文件添加到 `zh-cn` 文件夹中
>
> 另外需要在 `_sidebar.md` 文件中更新博客文件路径
>
> 如果不启用域名，需要删除 `CNAME` 文件

### 快速部署

在你的 `GitHub` 上创建一个仓库，随意命名，比如命名问 `x-blog`，然后修改 `jeremy-blog` 为 `x-blog` ，最后再在 `clone` 的基础上继续执行如下语句

```powershell
cd x-blog &&
git init &&
git add . &&
git commit -m "update and publish x-blog" &&
git branch -M main &&
git remote add origin git@github.com:[Your GitHub's Name]/x-blog.git &&
git push -f -u origin main
```

> **注意**
>
> 上面语句中 `[Your GitHub's Name]` 需要修改为你的 `GitHub` 账户名称

然后再在 `GitHub` 上配置界面修改 `Pages`信息即可

`Custome domain` 这里需要注意，默认是空的，如果你有域名可以在这里配置，如果没有保持默认即可

配置信息如下界面

![image-20220113163825749](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220113163825749.png)

这时候如果不出意外，就可以通过 `https://[Your GitHub's Name]/x-blog/` 进行访问了 :rocket:

### 项目地址 :gift:

GitHub: https://github.com/JeremyWu917/jeremy-blog

### 官网地址 :earth_africa:

Jeremy WU's Blog: https://blog.jeremywu.top



感谢阅读 :coffee: