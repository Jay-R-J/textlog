---
layout: post
title: "DeepSeek Harness 上手速查表：装好、启动、开玩"
permalink: deepseek-harness-onboarding-quick-start
categories: [技术速查, AI工具, 教程]
tags: [DeepSeek, DeepSeek Harness, DSH, AI工具, AI工作台, AI Agent, 插件框架, Cordis, 动态插件, 创造模式, 上手教程, 速查, AI编程助手, 命令行工具, AI操作电脑, 本地部署AI]
description: "DeepSeek Harness 极简上手速查表：安装命令、启动方式、核心概念、创造模式闭环、词表速查，10分钟从零到跑起来。"
author: Jay
---

> **这是极简上手速查表**，10分钟装好、启动、开聊。  
> 完整攻略（含动态插件、Agent预设、工作台所有玩法）请移步[完整版文章](https://www.jay-r-j.top/deepseek-harness-complete-guide/) 


---

## 一句话定位

> **DeepSeek Harness（DSH）** 是一个能让 AI 真刀真枪操作电脑的工作台，最绝的是它能**现场给自己加新能力**——你聊着聊着，它就能在网页里长出一个新面板、新工具，你点一下批准，它立刻就能用。

---

## 安装 & 启动（三选一）

**最省事：一行命令，连装都不用装**

```bash
npx --yes @deepseek-ai/dsh web
```

**装到全局，以后直接敲 dsh**

```bash
npm install -g @deepseek-ai/dsh
dsh web
```

**从源码构建（改源码/贡献代码才用）**

```bash
git clone https://github.com/deepseek-ai/deepseek-harness
cd deepseek-harness
pnpm install
pnpm build
```

启动后浏览器打开 http://127.0.0.1:3080。

---

### 常用命令速查

```bash
dsh web                                  # 启动网页界面
dsh --profile headless "把测试跑一遍"     # 无头模式，干完就退出
dsh --profile tui --patch ./extra.yml    # 终端界面 + 补丁层
dsh --profile tui --resume <会话id>       # 恢复某个会话
dsh plugin --profile tui add <包名>       # 给档案装插件
dsh --dump-config                        # 打印完整插件树（理解系统的最好入口）
dsh -V                                   # 版本号
```

---

## 三个核心概念（记住就能玩懂）

1. **Cordis 框架**：一切能力皆插件。工具、服务、提示词、按钮全是插件，写在一份 `cordis.yml` 清单里。
2. **宿主层 vs 预设层**：宿主层全系统共用（沙箱、审批、模型路由），预设层单个会话独享（工具、性格、开场白）。问自己一句"还有别的会话会用吗？"来区分。
3. **预设 = 人格 + 工具箱**：新建会话时选，选了就定死，中途不能换（因为换工具后历史消息里的 `tool_calls` 会校验失败）。

系统自带四个预设：`standard`（标准）、`code`（编程）、`minimal`（极简）、`cordis`（创造模式，最值得玩）。

---

## 创造模式闭环（DSH 最招牌的玩法）

新建会话时选 **创造模式**（cordis 预设），这个 AI 拥有了读写运行时组成的全部工具。你可以直接对它说：

> "创建一个只读审查预设，只能看代码不能改文件"

它会复制一份现有预设，按你的要求改好，存进 `~/.dsh/.agent-presets/`，然后这份新预设就出现在名单里了。你开个新会话选它，就多了一个专属的审查 AI。

这就是"让 AI 造 AI"，不是概念，是真能这么用。

> **底线**：创造模式 = shell 级信任，别跑不明来源的指令。

---

## 动态插件闭环

```
你：给我加一个显示 token 消耗的小面板
↓
AI 写一个动态插件（宿主代码 + 界面代码）
↓
你审批（弹窗高亮显示危险操作）
↓
装载进正在跑的程序
↓
网页上立刻出现这个面板，当场就能用
↓
不想要了：停止（界面消失、副作用全回收）或彻底删除
```

**关键机制**：

- **版本不可变**：每版锁死一个 `packageId`，新版只能追加不能覆盖
- **必须审批**：弹窗高亮 `fs`、`child_process`、`net` 等危险操作
- **作用域回收**：停止/删除时样式、定时器、事件监听全部撤干净

> **更轻量的替代方案**：如果不想写动态插件，用 **技能（Skills）**——纯文本 `SKILL.md` 文件挂进预设，AI 启动时当说明书读，零风险、永久生效。

---

## 目录结构速查

```
~/.dsh/
├── profiles/web/
│   ├── cordis.yml        # 档案清单（默认空）
│   └── cordis.patch.yml  # 你自己的补丁层，改配置写这里
├── sessions/             # 每个会话的记录（JSONL 日志）
├── settings.yaml         # 设置（默认预设等）
├── .credentials.yaml     # 模型密钥
└── .agent-presets/       # 你自建的 Agent 预设，一个目录一个
```

---

## 词表速查

| 词 | 什么意思 |
|---|---|
| DSH | DeepSeek Harness |
| Cordis | 插件框架，一切能力皆插件 |
| 预设 | 一个会话的 AI 组装方案（人格+工具箱） |
| 会话 | 一次对话和它的记录 |
| 宿主层 | 全系统公用的注册表和设施 |
| 动态插件 | 只活在当前进程的临时扩展 |
| Package | 动态插件的不可变版本 |
| Inspect | 查活体运行时契约的工具（让 AI 不猜 API） |
| MCP | 接外部工具服务器的协议 |
| Skills | 纯文本工作流说明书，挂进预设永久生效 |

---

## 三条底线

1. **创造模式** = shell 级信任，只跑信得过的指令。
2. **动态插件**装载前必审，重点看弹窗高亮的危险代码片段。
3. **随附预设**（`standard`/`code`/`minimal`/`cordis`）只读别改，升级会覆盖。

---

**完整攻略**（含工作台所有玩法、动态插件实战案例、Agent预设管理）：[完整版文章](https://www.jay-r-j.top/deepseek-harness-complete-guide/) 
