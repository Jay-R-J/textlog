---
layout: post
title: "Jekyll textlog 部署到 GitHub Pages 极简速查表"
permalink: jekyll-textlog-github-pages-cheatsheet
categories: 教程
tags: [Jekyll, GitHub Pages, 极简博客, 速查, Windows, 博客搭建, Ruby, textlog, 部署教程, 免费博客, 静态网站, 个人博客]
description: "Jekyll textlog 主题部署到 GitHub Pages 的极简命令速查表，Windows 环境下一键搭建免费个人博客。"
author: Jay
---

> **这是极简部署速查表**，专供遇到报错时快速复制命令。  
> 新手小白/想看完整图文详解（含每步截图）请移步[完整保姆级教程](https://www.jay-r-j.top/textlog-github-pages-tutorial/)


---

## 一键命令清单

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

---

## 关键配置速查（_config.yml 中的 baseurl）

| 你的仓库名 | baseurl 应设为 | 最终访问地址 |
|-----------|---------------|-------------|
| 用户名.github.io | `""`（空字符串） | `https://用户名.github.io/` |
| 其他（如 textlog） | `"/仓库名"` | `https://用户名.github.io/仓库名/` |

> **baseurl 必须开头有斜杠，末尾无斜杠**，且与仓库名完全一致（大小写敏感）。

---

## 避坑三连（最常犯的错误）

### 1. Ruby 版本不对

必须使用 **Ruby 2.7.8**（带 DevKit），3.x 会报 `untaint` 错误。安装路径不能有空格（别装到 `Program Files` 或含中文的目录）。

### 2. baseurl 写错

仓库名是 `textlog` 就必须填 `"/textlog"`。填错或漏填会导致线上样式完全加载失败。

### 3. 本地访问地址错误

本地预览后，请用终端最后一行显示的地址（如 `http://127.0.0.1:4000/textlog/`）打开，而不是想当然地只输 `4000`。

---

## 我实测通过的环境版本

- **Ruby** 2.7.8（带 DevKit）
- **Bundler** 1.17.3
- **主题**：heiswayi/textlog
- **部署**：GitHub Pages（main 分支）

---

## 完整图文教程

[点击这里查看完整教程（含每步截图 & 所有疑难解答）](https://www.jay-r-j.top/textlog-github-pages-tutorial/)
