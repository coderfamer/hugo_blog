---
title: "Hugo"
date: 2022-12-02T00:20:59+08:00
draft: false
---
# hugo 安装和使用

## hugo 安装

## 建立站点

```shell
hugo new site hugo_blog
```

## 添加主题 

```shell
cd hugo_blog
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

配置文件中使用指定主题

```shell
echo 'theme = "ananke"' >>config.toml
```

## 创建新文章

```shell
hugo new posts/my-first-post.md
```

```shell
---
title: "My First Post"
date: 2022-12-02T00:00:08+08:00
draft: false
---

## first post
```

## 开启本地服务

```shell
hugo server --bind=0.0.0.0 --baseURL=http://<yourhost>:1313
```



# 在 github 部署个人博客

使用 github 的 user pagers 功能创建 



  coderfamer.github.io

```shell
rm -rf public
git submodule add -b master git@github.com:<yourgitname>/<yourgitname>.github.io.git public
```

在根目录的 config.toml 添加 baseURL

```shell
baseURL = 'https://<youergit>.github.io/'
```

生成静态网页，提交代码

```shell
hugo 
cd public
git add --all
git commit . "first post"
git push origin master
```

