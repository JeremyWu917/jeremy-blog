# 极客必备 - 教你搭建免费图床

> 图床是什么？
>
> 图床是指 **存储图片的服务器** ，可将图片上传至图床并得到一个访问图片的 `url` 链接。
>
> 随着图床的概念越来越流行，图床的功能也越发强大。在原有图片存储和生成访问链接的基础上，通常还提供 `CDN` 加速、`URL` 上传、图片压缩、图片裁切、图片水印、图片鉴黄等功能。
>
> 但毕竟图床会占用服务器资源，因此很多图床服务都是收费的。对于临时使用、图片数不多且图片无隐私的用户，更希望能免费使用图床。

### 为什么要使用图床？

- 为了减少服务器的压力，大多数站点管理员都会选择将一些静态资源存储到团床
- 多端引用，特别是多余极客而言，当需要将博文发布到不同的博客网站时，如果不采用图床，将会非常的麻烦

### 常用的图床对比与分析

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    作为互联网流浪民工，今天就根据个人使用经验来为大家简单对比一下常用的图床
</section>

#### 免费图床

##### 路过图床

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    功能非常强大的免费图床。这也是目前我在使用的图床之一
</section>

适用场景：个人用户存储图片

优点：

1. 已上线多年，服务较稳定
2. 上传操作简单
3. 功能强大，上传图片后能得到各种格式的图片链接（如 `HTML`、`Markdown`）
4. 有 `CDN` 加速

缺点：

1. 图片公开存储，安全性不高
2. 部分有时候外链失效，比如微信公众号

地址：https://imgchr.com/

![image-20220115103328677](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115103328677.png)

#####  SM.MS 图床

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    简单易用的图床，提供 5G 的免费空间，可付费开通更多资源
</section>

适用场景：个人用户存储小图片

优点：

1. 已上线多年，服务较稳定
2. 上传操作简单
3. 支持作为 `PicGo` 的图床

缺点：

1. 免费空间略小
2. 国内用户访问较慢

地址：https://sm.ms/

![image-20220115103546009](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115103546009.png)

#####  ImgURL 图床

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    简单易用的图床，但是游客限制每日上传 10 张
</section>

适用场景：适合个人用户临时存储图片

优点：

1. 上传操作简单
2. 可以独立部署

缺点：

1. 免费上传数太少

地址：https://imgurl.org/

![image-20220115103802098](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115103802098.png)

##### Gitee 和 GitHub

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    Gitee 和 GitHub 都是免费的代码托管平台，也可以将图片像代码一样进行托管在他们的服务器上，实现图床的存储功能
    <br>
    如果您要上传的文件不超过 1M 更推荐使用 Gitee，国内用户访问速度更快更稳定，这个也是我目前用的比较多的图床之一了
</section>

适用场景：适合开发者或企业长期存储图片

优点：

1. 完全免费
2. 可以建立私有仓库，较安全
3. 对开发者友好，可以通过版本控制系统（`Git`）来管理图片

不足：

1. 由于平台不是专门提供图床服务的，因此没有图片裁切、水印等高级功能。
2. 对于从来没使用过代码托管平台的同学，需要一定的学习成本。

Gitee 地址：https://gitee.com/ 

GitHub 地址：https://github.com/

![image-20220115105543121](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115105543121.png)

![image-20220115105637613](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115105637613.png)

#### 收费图床

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0;">
    氪金的产品就不做对比了，反正我用起来都不错
</section>

##### 阿里云 OSS

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    阿里系的产品还是值得信赖的，看下这个配置和这个价格，还要啥自行车！特别是搭配域名使用，直接飞起
</section>

地址：https://common-buy.aliyun.com/

![image-20220115104746942](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115104746942.png)

##### 七牛云 KODO

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    七牛云提供国内领先的云存储服务，可以通过其对象存储产品来作为图床
</section>

地址：https://www.qiniu.com/prices/kodo

![image-20220115105008169](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115105008169.png)

##### 腾讯云 COS

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    企鹅系的产品也是不遑多让，看下这个配置和这个价格，对标阿里不是问题，整套解决方案不是问题
</section>

地址：https://cloud.tencent.com/act/pro/cos

![image-20220115110344981](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115110344981.png)

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6;">
    氪金虽好，但是要量力而行，作为个人学习和写博客就没有必要氪金了，免费的图床足够使用。
</section>

### 使用图床

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7;">
    以 Gitee 为例讲解一下如何使用图床，对于习惯使用 markdown 书写的小伙伴在搭载 Typora 和 PicGo 之后直接起飞
</section> 
<br>
<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0;">
    图片大小不要超过 1M 否则会受到影响
</section>

##### 创建 Gitee 账户

首先，如果您没有 `Gitee` 账户，您需要先注册一个账户

`Gitee` 地址：https://gitee.com/

##### 创建图床仓库

如何创建仓库就不再展开了，需要注意一点，仓库必须是公开的。

##### 创建 `token`

点击个人头像\设置\私人令牌\生成新令牌，填写令牌名称，然后勾选 `user_info` 和 `projects` 即可，**一定不要保持默认直接提交**，权限够用即可，不需要所有的权限。

点击复制按钮，这就是接下来我们需要用到的 `token`

![image-20220115115101083](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115115101083.png)

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6;">
    默认情况下此令牌拥有该 Gitee 账户的所有仓库和个人用户信息的操作权限，注意信息安全，不要外泄。
</section>

#### 安装 PicGo 并配置

##### 安装

下载并安装 `PicGo`，注意安装路径，后续需要用到路径

下载地址：https://github.com/Molunerfinn/PicGo/releases

安装 `picgo uploader for gitee` 插件

![image-20220115114221449](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115114221449.png)

##### 图床设置

- `owener` - 填写 `Gitee` 用户名
- `repo` - 填写项目名称
- `path` - 填写项目路径，可以为空（注意这里的路径在 `Gitee` 仓库中一定要存在）
- `token` - 填写上一步复制的 `Gitee` 上获取的 `token`
- `message` - `push` 时的备注信息

![image-20220115114320257](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115114320257.png)

#### 安装 Typora 并配置

##### 安装

下载并安装 `typora`

下载地址： [http://typora.io](http://typora.io/)

##### 配置

点击 `File\Preferences\Image` ，选择 `PicGo` 可执行文件的路径保存即可。

![image-20220115120725944](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/image-20220115120725944.png)

#### 运行展示

![picgo-202201151216](https://raw.githubusercontent.com/jeremywu917/jeremywuassets/main/src/blog/picgo-202201151216.gif)

感谢阅读 :coffee: