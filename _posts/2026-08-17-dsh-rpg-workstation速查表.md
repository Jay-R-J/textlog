---
layout: post
title: "dsh-rpg-workstation 速查表：把 DeepSeek Harness 变成 RPG 工作台"
permalink: dsh-rpg-workstation-speed-guide
description: "dsh-rpg-workstation 极简速查表：一条命令安装、经验公式、连击加成、14个成就、面板交互，5分钟让你的 DSH 变成 RPG 工作台。"
categories: 技术速查
tags: [DeepSeek Harness, RPG工作台, 插件, 速查, 游戏化]
author: Jay
---

> **这是极简速查表**，5分钟装好开玩。  
> 完整玩法攻略（含设计思路、架构详解、玩法建议）请移步[完整版文章](https://www.jay-r-j.top/dsh-rpg-workstation-plugin-guide/) 

---

## 一句话定位

> **dsh-rpg-workstation** 是一个让 DeepSeek Harness 长出 RPG 面板的插件。每完成一个对话回合获得经验值，升级、解锁 14 个成就、保持每日连击，右下角浮动面板实时展示战绩。

**作者**：Jay（就是我本人），MIT 协议开源。  
**仓库**：`https://github.com/Jay-R-J/dsh-rpg-workstation`

---

## 安装（一条命令）

**前置条件**：已全局安装 DSH CLI（`npm install -g @deepseek-ai/dsh`）

```bash
dsh plugin --profile web add dsh-rpg-workstation
```

装完后重启 `dsh web`（Ctrl+C 停掉，再 `dsh web` 重新启动），右下角就会出现 RPG 面板。

**卸载**：

```bash
dsh plugin --profile web remove dsh-rpg-workstation
```

---

## 核心公式速查

| 项目 | 公式 |
|------|------|
| 每回合经验 | `(10 + min(20, 工具数 × 2)) × 连击加乘` |
| 连击加乘 | 3天 ×1.1 / 7天 ×1.25 / 30天 ×1.5 |
| 等级 | `level = floor(sqrt(xp / 100)) + 1` |
| 升下一级阈值 | `level² × 100` |
| 称号 | 1-2新手 / 3-4学徒 / 5-6冒险者 / 7-9勇者 / 10-14大师 / 15+传说 |

> **关键细节**：回合必须正常完成（`turn/end` + `completed`）才计分；工具加成单回合上限 +20；连击是乘法不是加法，保持每天聊一句比某天猛聊划算得多。

---

## 面板交互

| 操作 | 效果 |
|------|------|
| 单击面板头部 | 折叠或展开主体 |
| 按住头部拖动 | 任意位置移动 |
| 单击成就圆点行 | 展开详情区 |
| 点击「导出」 | 下载存档 `rpg-data-YYYY-MM-DD.json` |
| 点击「导入」 | 选择 JSON 文件恢复存档 |

详情区包含：今日任务（3个）、签到日历（近7天）、本周报告（柱状图+汇总）、统计（工具调用/文件写入/今日回合）、14个成就列表、导入导出。

---

## 架构概览

- **Host 面**（`src/index.ts`）：监听会话事件、算分、落盘、提供 3 个路由（state/export/import）
- **Client 面**（`src/client/`）：插槽注册（shell.overlay）、样式注入、轮询渲染
- **持久化**：`~/.dsh/rpg-data.json`，原子写入，进程崩溃不损坏存档
- **主题适配**：自动跟随宿主深色/浅色，无 emoji 噪音，支持 `prefers-reduced-motion`

---

## 14 个成就一览

| 成就 | 解锁条件 |
|------|----------|
| 初次对话 | 完成第 1 个回合 |
| 对话达人 | 累计 100 个回合 |
| 对话大师 | 累计 1000 个回合 |
| 等级突破 Lv.5 | 达到 5 级 |
| 等级突破 Lv.10 | 达到 10 级 |
| 等级突破 Lv.15 | 达到 15 级 |
| 工具初学者 | 累计调用 10 次工具 |
| 工具爱好者 | 累计调用 100 次工具 |
| 工具狂人 | 累计调用 1000 次工具 |
| 文件写入者 | 累计写入 10 个文件 |
| 文件编辑者 | 累计写入 100 个文件 |
| 连击 3 天 | 连续 3 天活跃 |
| 连击 7 天 | 连续 7 天活跃 |
| 深夜/清晨档 | 在凌晨 0-5 点完成一个回合 |

> 全成就解锁时，称号变为「圆满」，卡片出现高亮彩蛋。

---

## 存档位置

```
~/.dsh/rpg-data.json
```

纯 JSON 格式，数据完全归你。可导出备份，也可导入恢复。

---

**完整版**（含设计思路、玩法建议、架构详解、与前作呼应）：[点击这里](https://www.jay-r-j.top/dsh-rpg-workstation-plugin-guide/)

