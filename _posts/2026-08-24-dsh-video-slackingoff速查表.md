---
layout: post
title: "dsh-video-slackingoff速查表：AI思考时自动弹短视频，想完自动关"
permalink: dsh-video-slackingoff-plugin-tutorial
categories: 技术速查
tags: [DeepSeek Harness, 摸鱼插件, 短视频, dsh插件, 速查, AI工具]
description: "dsh-video-slackingoff 极简速查表：一条命令安装，AI思考时自动弹短视频窗口，思考结束自动关闭。支持快手/抖音切换，状态持久化，摸鱼不用记着关。"
author: Jay
---

> **这是极简速查表**，只保留安装命令和核心交互。
> 完整开发记（含痛点分析、轮询退避设计、window.open踩坑记录）请移步 [https://www.jay-r-j.top/dsh-video-slackingoff-plugin-guide/](https://www.jay-r-j.top/dsh-video-slackingoff-plugin-guide/)

---

## 一句话定位

> **dsh-video-slackingoff** 是一个让 DeepSeek Harness 在 AI 思考时自动弹出短视频窗口、思考结束自动关闭的插件。摸鱼不用记着关，干活不被打扰。

**作者**：Jay（就是我本人），MIT 协议开源。
**仓库**：[https://github.com/Jay-R-J/dsh-video-slackingoff](https://github.com/Jay-R-J/dsh-video-slackingoff)

---

## 安装（一条命令）

**前置条件**：已全局安装 DSH CLI（`npm install -g @deepseek-ai/dsh`）

```bash
dsh plugin --profile web add dsh-video-slackingoff
```

装完后重启 dsh web（Ctrl+C 停掉，再 dsh web 重新启动），右下角会出现状态药丸。

**卸载**：

```bash
dsh plugin --profile web remove dsh-video-slackingoff
```

---

## 状态药丸速查

右下角药丸实时反映当前状态：

| 显示 | 颜色 | 含义 |
|---|---|---|
| 摸鱼中 | 绿 | AI 正在思考，窗口已弹出 |
| 摸鱼 开 | 灰 | 已开启，AI 没在思考（待机） |
| 摸鱼 关 | 灰 | 总开关关了，不弹窗 |
| 弹窗被拦 | 黄 | 浏览器拦截了，去地址栏允许一次 |

---

## 交互方式

| 操作 | 效果 |
|---|---|
| 点击药丸 | 切换总开关（开 ↔ 关），状态自动保存 |
| 点击「选视频」按钮 | 弹出视频源列表（快手 / 抖音） |
| 点选某个视频源 | 下次自动用这个，存在 localStorage |

---

## 三个关键设计

- **弹窗被拦自动重试**：浏览器首次会拦截非用户手势触发的弹窗，去地址栏允许一次后，本轮内自动恢复。
- **用户误关窗口自动重开**：轮询 tick 检测到窗口被误关，自动重开，不会出现"本轮思考剩半截没窗口"的死局。
- **轮询退避**：AI 思考时 500ms 查一次状态（不延迟），空闲时 2s 查一次（省请求）。

---

## 持久化

- **总开关状态**：保存在 ~/.dsh/video-slackingoff.json（尊重 DSH_HOME 环境变量）
- **视频源选择**：保存在浏览器 localStorage

---

**完整开发记**（含痛点由来、window.open踩坑、轮询退避设计）：[https://www.jay-r-j.top/dsh-video-slackingoff-plugin-guide/](https://www.jay-r-j.top/dsh-video-slackingoff-plugin-guide/)
