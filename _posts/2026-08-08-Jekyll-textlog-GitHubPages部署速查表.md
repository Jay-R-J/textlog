---
title: "Jekyll textlog 部署 GitHub Pages 极简速查表 — Windows 一键搭建博客"
categories: 教程
tags: [Jekyll, GitHub Pages, 极简博客, 速查, Windows, 博客搭建, textlog, Ruby, 部署, 免费博客]
description: "Jekyll textlog 主题部署到 GitHub Pages 的极简命令速查表，Windows 环境下一键搭建免费个人博客。"
---

> **这是极简部署速查表**，专供遇到报错时快速复制命令。  
> 新手小白/想看完整图文详解（含每步截图）请移步[完整保姆级教程](https://www.jay-r-j.top/textlog-github-pages-tutorial/)  
> **（请先将完整版发布后，再把此链接替换为真实地址）**

---

## ⚡ 一键命令清单

```powershell
# 1. 克隆主题
git clone https://github.com/[你的GitHub用户名]/[仓库名].git

# 2. 安装 Bundler（必须 1.17.3 版本）
gem install bundler:1.17.3

# 3. 安装所有依赖
bundle _1.17.3_ install

# 4. 本地预览（注意用终端显示的地址访问）
bundle _1.17.3_ exec jekyll serve

# 5. 提交上线
git add .
git commit -m "更新博客"
git push origin main
```
