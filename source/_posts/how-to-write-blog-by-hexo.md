---
title: 怎样优雅地写技术博客
date: 2017-12-13 15:39:35
tags: writing
---

# 前言

写技术博客，很简单，难在坚持 :)

整体来说，写技术博客，只需要，基本的 markdown 语法，一个合适的博客框架，一台服务器，一个公网 IP，一个域名，一个社交评论账号。就是这么简单。

# 准备

node 环境 [node dmg](https://nodejs.org/en/download/)

markdown 编辑器 [macdown](https://macdown.uranusjr.com/)

![macdown](https://macdown.uranusjr.com/static/images/macdown-demo.png)

屏幕 gif 录制 [gifox](https://gifox.io/)

![gofix](https://gifox.io/image/free-selection@2x.jpg)

屏幕 video 录制 [screenflow](https://www.telestream.net/screenflow/)

![screenflow](http://soft.macx.cn/upload/softicon1/201211301376.jpg)

服务器 [阿里云](https://www.aliyun.com/product/ecs) 或 [aws](https://aws.amazon.com/ecs)

博客框架 [hexo](https://hexo.io)

# Hexo

## 写作

安装 hexo

```
npm install hexo-cli -g
```

创建博客项目

```
hexo init blog
```

创建新文章

```
hexo new article
```

编译新文章

```
hexo generate
```

本地查看文章

```
hexo server
```
	
## 评论


注册 [disque](https://disqus.com/) （注意，使用 disque 需要 FTW）
	
记住 自己博客网站在 disque 的 shortname，一般在这里能看到：

在 hexo 项目中的主题 config.yml 进行 disque 的相应配置：

```
# disqus
disqus:
  enable: true
  shortname: springmarchcom
  count: false

# disqus
url: http://springmarch.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

## 图片

安装图片组件

```
npm install hexo-asset-image --save
```

在 hexo 项目的 config.xml 进行图片配置

```
post_asset_folder: true
```

如何使用？在 source 下，hexo asset image 默认创建和文章同名的文件夹，将引用的图片存放在那里，即可

```
# 图片存放
louis@mac ~/workspace/personal/hexo-blog/source/_posts tree -L 2
.
├── how-to-write-blog-by-hexo
│   ├── disque.jpg
│   ├── disque_hexo_config.jpg
│   ├── screenflow.gif
│   └── show_gofix.gif
├── how-to-write-blog-by-hexo.md

# Markdown 中引用
![disque_config](disque_hexo_config.jpg)
```

注意，如果文章文件是中文，可能会有问题。

具体参考 [CodeFalling/hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image)

## 服务器与域名

域名可以在 godaddy 购买。

建议购买国外的阿里云服务器，省了备案。（有时，国外线路会出问题，故障时间最长一周左右，你知道的，但可以忍受，发工单骚扰阿里云的兄弟问问什么情况就心里有数了）

购买服务器后，在服务器生成 ssh key，并在 [github](https://github.com/settings/keys) 上配置 ，以便部署时，hexo-deployer-git 组件有权限访问服务器。

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

最后，在服务器上预设博客文件路径和 nginx 反代理配置：

```
# 笔者为例，博客文件夹设为
/opt/www/hexo_blog

# 对应的 nginx 配置为
server
{
    listen 80;
    server_name www.springmarch.com springmarch.com;
    index index.html index.htm default.html default.htmp;
    root  opt/www/hexo_blog;

    ...

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## 部署

添加部署组件

```
npm install hexo-deployer-git --save
```

在 hexo 项目的 config.xml 进行部署配置

```
deploy:
    type: git
    repo: git@xx.xx.xx.xx:/opt/www/hexo_blog.git
    branch: master           
    message: 更新博客
```

其中，repo 中的 xx.xx.xx.xx 为服务器公网 ip，/opt/www/hexo_blog 为上传博客静态资源的路径。

提交更新到 github 仓库:

```
louis@mac ~/hexo-blog git commit -am "upate header"
...
 
louis@mac ~/hexo-blog master git push origin master
...
```
   
部署到服务器 

```
louis@mac  ~/workspace/personal/hexo-blog hexo deploy
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
On branch master
nothing to commit, working tree clean
Everything up-to-date
Branch master set up to track remote branch master from git@47.52.95.8:/home/git/hexo_blog.git.
INFO  Deploy done: git
```

完毕，这时候博客文章就发表出去了。


# 其他

文章，少用图片，第一编写不顺畅，第二图片大小是个问题，可以用外链或者不用图片描述问题。大家有更方便的流程可以留言告知。（例如能将 gif 进行压缩的免费图床）

发现图片加载慢时，会影响 js 对 markdown 片段的呈现，用了[图片懒加载](https://www.npmjs.com/package/hexo-lazyload-image)，暂时除了美观度和并发加载数只有3个不满外，还能接受。
