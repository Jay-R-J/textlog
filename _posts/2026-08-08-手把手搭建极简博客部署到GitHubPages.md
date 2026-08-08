---
layout: post
title: "手把手搭建极简博客：使用 heiswayi/textlog 主题部署到 GitHub Pages"
tags: [极简博客, GitHub Pages, 教程, 博客搭建]
author: Jay
categories: 教程
comment: true
---

本文记录了我从零开始，在 Windows 下使用 heiswayi/textlog 极简 Jekyll 主题，搭建个人博客并部署到 GitHub Pages 的完整流程。

无论你是首次搭建，还是已有旧站想并存，都能找到适合自己的方式。文中包含了我在过程中遇到的所有坑和解决方案，让你一次成功。

---

## 最终效果

- **首次搭建**：博客访问地址为 `https://你的用户名.github.io/`，干净简洁
- **已有旧站**：旧站 `https://你的用户名.github.io/` 保持不变，新博客访问 `https://你的用户名.github.io/仓库名/`
- 所有文章以 Markdown 书写，极简、无干扰、支持 RSS

---

## 🛠 准备工作

- **Git**：[下载安装（官网）](https://git-scm.com/)
- **GitHub 账号**（已登录）
- **Windows 操作系统**（Win10/11 均可）
- 一个**纯英文**的本地工作目录（例如 `D:\MyBlog`），避免路径中含有空格或中文（否则编译扩展时会出错）

---

## 第一步：创建 GitHub 仓库并克隆主题

### 1. Fork 主题仓库

访问 [heiswayi/textlog](https://github.com/heiswayi/textlog)，点击右上角 **Fork**，将仓库复制到你的 GitHub 账号下。

![Fork截图](/textlog/img/1/fork_image.png)

### 2. 重命名仓库（关键！请根据你的情况选择）

**场景 A（推荐，适合首次搭建）**：如果你尚未占用 `用户名.github.io` 这个仓库，请将 Fork 后的仓库重命名为 `用户名.github.io`（将"用户名"换成你的 GitHub ID）。

这样博客将直接部署在根域名，访问地址为 `https://用户名.github.io/`。

**场景 B（已有旧站）**：如果你的 `用户名.github.io` 已经被占用（例如已有博客或项目），请取一个其他名字，比如 `textlog` 或 `my-blog`。

新博客访问地址为 `https://用户名.github.io/仓库名/`，与旧站互不干扰。

> 记住你最终选定的仓库名，后面配置 `_config.yml` 中的 `baseurl` 时要用到。

### 3. 克隆到本地（使用纯英文路径）

打开 PowerShell 或 CMD，执行以下命令（以仓库名为 textlog 为例，请替换成你自己的仓库名）：

```powershell
cd D:\
mkdir MyBlog
cd MyBlog
git clone https://github.com/你的用户名/textlog.git
cd textlog
```

---

## 第二步：配置 _config.yml

用记事本（或 VS Code）打开根目录下的 `_config.yml`，修改以下关键字段：

```yaml
title: "我的极简博客"          # 你的博客标题
description: "记录思考和日常"   # 站点描述
baseurl: ""                    # 重要！根据你的仓库名填写（见下方说明）
url: "https://你的用户名.github.io"   # 不要加斜杠
author: "你的名字"

# 其他保持默认（如 google_analytics 留空即禁用）
```

### baseurl 填写规则（必看）：

| 你的仓库名 | baseurl 应设为 | 最终访问地址 |
|------------|---------------|-------------|
| 用户名.github.io | `""` （空字符串） | `https://用户名.github.io/` |
| textlog | `"/textlog"` | `https://用户名.github.io/textlog/` |
| my-blog | `"/my-blog"` | `https://用户名.github.io/my-blog/` |

**核心原则**：`baseurl` 必须等于 `/仓库名`，开头有斜杠，末尾无斜杠。

仅当仓库名是 `用户名.github.io` 时，`baseurl` 必须设为空字符串。写错会导致线上样式完全加载失败。

---

## 第三步：安装 Ruby 和依赖（避坑重点）

### 3.1 安装 Ruby（版本必须正确）

我最初用最新版 Ruby（3.x）导致 Bundler 报 `untaint` 方法错误，所以请务必安装 **Ruby 2.7.8 带 DevKit**。

1. 下载 [Ruby 2.7.8 带 DevKit](https://rubyinstaller.org/downloads/)（不知道下哪个？[点这里通过百度网盘下载](https://pan.baidu.com/s/1K3MM0lb1K7WMECQgWmjQnQ?pwd=jdnh)，提取码：jdnh）
2. 安装时务必勾选：
   - "Add Ruby to PATH"
   - "MSYS2 development toolchain"
3. 安装路径必须**纯英文且无空格**，例如 `C:\Ruby27`（不要用 `C:\Ruby + Jekyll + Bundler`，否则编译扩展时会因空格导致 make 失败）
4. 安装完成后，**重启计算机**（或至少注销重新登录）让环境变量生效。

![Ruby安装截图](/textlog/img/1/ruby.png)

### 3.2 安装 Bundler 并安装依赖

打开新的 PowerShell，进入项目目录：

```powershell
cd D:\MyBlog\textlog
gem install bundler:1.17.3
bundle _1.17.3_ install
```

如果一切顺利，你会看到一堆 gem 安装成功的信息。

如果看到 `http_parser.rb` 编译失败，检查 Ruby 安装路径是否含空格，并确认已安装 DevKit。

---

## 第四步：本地预览

```powershell
bundle _1.17.3_ exec jekyll serve
```

等待编译完成，终端会显示类似：

```
Server address: http://127.0.0.1:4000/textlog/
```

请务必按终端显示的地址访问（如果 `baseurl` 为空，则地址为 `http://127.0.0.1:4000/`）。

打开浏览器访问该地址，你应该能看到博客页面，且样式完整。

![本地预览截图](/textlog/img/1/result.png)

---

## 第五步：推送到 GitHub 并启用 Pages

### 1. 提交修改

在项目目录下执行：

```bash
git add .
git commit -m "初始化极简博客"
git push origin main
```

（如果你的默认分支是 `gh-pages`，请先改为 `main`，方法见常见问题）

### 2. 启用 GitHub Pages

- 进入你的 GitHub 仓库（例如 `https://github.com/你的用户名/textlog`）
- 点击 **Settings → Pages**
- 在 "Branch" 下选择 `main` 分支，目录选 `/ (root)`，点击 **Save**

### 3. 等待 1~3 分钟

访问对应的地址（根据你的仓库名）：

- 若仓库名为 `用户名.github.io`：`https://用户名.github.io/`
- 若仓库名为 `textlog`：`https://用户名.github.io/textlog/`

线上博客即可正常访问。

---

## 常见问题与解决方案

### 问题1：bundle install 报 `untaint' for String (NoMethodError)`

- **原因**：Ruby 版本太新（3.x），Bundler 1.17.3 不兼容。
- **解决**：卸载当前 Ruby，安装 Ruby 2.7.8（带 DevKit），并确保路径无空格。

### 问题2：安装 http_parser.rb 时 make failed, exit code 2

- **原因**：Ruby 安装路径含有空格（如 `Ruby + Jekyll + Bundler`），导致 Makefile 解析错误。
- **解决**：重装 Ruby 到纯英文无空格路径（如 `C:\Ruby27`）。

### 问题3：本地预览样式错乱（只有文字）

- **原因**：`_config.yml` 中的 `baseurl` 与仓库名不一致，或访问地址忘了加 `/仓库名/`。
- **解决**：检查 `baseurl` 是否按速查表正确填写，访问时使用终端显示的实际地址。

### 问题4：线上访问样式丢失

- **原因**：GitHub Pages 未生效，或 `baseurl` 配置错误。
- **解决**：等待几分钟，强制刷新（Ctrl+F5）；确认仓库名与 `baseurl` 一致。

### 问题5：MSYS2 安装时出现 GPG 超时警告（Connection timed out）

- **原因**：网络问题，但密钥未变。
- **解决**：忽略即可，不影响 Ruby 和 Jekyll 使用。

### 问题6：默认分支不是 main 而是 gh-pages

- **解决**：在 GitHub 仓库 Settings → Branches 中将默认分支改为 `main`，然后在本地执行 `git branch -m gh-pages main`，再推送。

---

## 补充：如何写第一篇博客

文章存放在 `_posts` 文件夹，文件命名格式为 `YYYY-MM-DD-标题.md`，例如 `2026-08-08-我的第一篇文章.md`。

文件开头需包含以下 Front Matter：

```yaml
---
layout: post
title: "我的第一篇文章"
date: 2026-08-08
---
```

之后用 Markdown 书写正文即可。

---

## 结语

至此，你已经拥有一个属于自己的极简博客，完全免费，数据自主。

heiswayi/textlog 主题干净、无干扰，让你专注于写作本身。如果你遇到本文未提及的问题，欢迎在评论区留言，我会尽力解答。

Happy Blogging！
