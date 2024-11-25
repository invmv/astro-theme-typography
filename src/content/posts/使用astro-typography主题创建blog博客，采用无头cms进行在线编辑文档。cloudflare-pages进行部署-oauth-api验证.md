---
title: 使用Astro typography主题创建Blog博客，采用无头CMS进行在线编辑文档。Cloudflare Pages进行部署 Oauth API验证
pubDate: 2024-11-25
description: 本站同款建站方式 前端 Astro-typography、decapcms、后端部署Cloudflare
  Pages、验证登录Github with OAuth
categories:
  - 闲来无事
---
# 前言

博客本是为了随手的记录，然而繁杂的配置、每次更新文章的痛苦、服务器/域名续费。让人心力交瘁，本文介绍最新的整合方案，让维护容易，配置简单。每次更新文章跟发朋友圈一样，填入标题，输入正文即可

# 准备工作

你需要一点点基础知识，可能不会手把手。

如果你只是想看如何配置decap cms，可以跳过主题配置

Github 账号 - Free

Cloudflare 账号 - Free

主题 [Moeyua/astro-theme-typography](https://github.com/Moeyua/astro-theme-typography)

Oauth：[i40west/netlify-cms-cloudflare-pages](https://github.com/i40west/netlify-cms-cloudflare-pages)

# 配置并部署仓库

打开主题并fork到自己仓库，fork是为了以后方便更新主题。fork完无需配置即可部署

前往Cloudflare pages创建一个Pages程序，使用刚刚Fork的仓库。

如无错误，应该会提示成功并且有Pages.dev结尾的链接供访问

如果需要自定义域名，请自定义域名先，稍后不用再次修改配置

# 配置Decap CMS

验证程序采用 https://github.com/i40west/netlify-cms-cloudflare-pages

创建一个新的Oauth应用程序 https://github.com/settings/developers

* Application name 随意填写
* Homepage URL 填写博客的主页
* Authorization callback URL 填写博客的主页

![](/uploads/clipboard-image-1732539090.webp)

## 创建完成进入应用，会看到Client ID和Client secrets。记录下来，

![](/uploads/clipboard-image-1732539271.webp)

## 下载cms程序到博客 https://github.com/i40west/netlify-cms-cloudflare-pages

functions 文件夹放到根目录

admin 文件夹放到 /pubilc

![](/uploads/clipboard-image-1732539512.webp)

## 修改config.yml

这是我的config.yml配置，按需更改

主要修改 repo，为你的fork的仓库 用户名/仓库名

base_url 为你设置oauth的主页地址

```yml
backend:
  name: github
  repo: user/repo
  branch: main # Branch to update (optional; defaults to master)
  base_url: https://invmy.us.kg
  auth_endpoint: /api/auth
media_folder: public/uploads
public_folder: /uploads
collections:
  - name: "blog"
    label: "Posts"
    folder: "src/content/posts"
    create: true
    slug: "{{slug}}"
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Publish Date", name: "pubDate", widget: "datetime", format: "YYYY-MM-DD" }
      - { label: "Description", name: "description", widget: "string", hint: "附言" }
      - label: "Categories"
        name: "categories"
        widget: "select"
        options: 
          - 交易日记
          - 经验分享
          - 数字移民
          - 闲来无事
        multiple: true # 允许多选
      - { label: "body", name: "body", widget: "markdown", modes: ["raw","rich_text"] }
```

## 保存并上传会自动重新部署，如无错误你在主页后面加/admin即可进入后台
