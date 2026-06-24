# 🤖 Claude Code + DeepSeek API 部署指南

[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-blue)](https://github.com/KingandWorld/awesome-ai-resources)
[![Node](https://img.shields.io/badge/node-18%2B-brightgreen)](https://nodejs.org)
[![License](https://img.shields.io/badge/license-MIT-green)](https://opensource.org/licenses/MIT)

> 零基础，30 分钟，用 DeepSeek API 驱动 Claude Code，花费每天不到一毛钱。

---

## 📖 目录

- [这篇教程适合谁](#这篇教程适合谁)
- [前置知识检查](#前置知识检查)
- [小白必读：这些是什么](#小白必读这些是什么)
- [第一步：注册 DeepSeek & 获取 API Key](#第一步注册-deepseek--获取-api-key)
- [第二步：安装 Node.js 和 Git](#第二步安装-nodejs-和-git)
- [第三步：安装 Claude Code](#第三步安装-claude-code)
- [第四步：配置 Claude Code 使用 DeepSeek](#第四步配置-claude-code-使用-deepseek)
- [第五步：启动 & 验证](#第五步启动--验证)
- [常见问题 FAQ](#常见问题-faq)
- [进阶技巧](#进阶技巧)
- [名词解释](#名词解释)

---

## 这篇教程适合谁

| 你的情况 | 适合吗 |
|----------|:---:|
| 想用 AI 写代码，但嫌 ChatGPT/Claude 订阅太贵 | ✅ |
| 听说过 Claude Code 但不知道怎么装 | ✅ |
| 对命令行不太熟悉，但愿意跟着教程一步步来 | ✅ |
| 已经装好了但连不上，想排查问题 | ✅ |
| 已经是熟练开发者，只想看配置模板 | ✅（直接跳到[第四步](#第四步配置-claude-code-使用-deepseek)） |

---

## 前置知识检查

> 别被「前置知识」吓到——下面这些你**不需要提前会**，只是帮你判断这篇教程适不适合你。

| 你会的东西 | 需要程度 |
|-----------|:---:|
| 会用浏览器上网 | 必需 |
| 会打字、复制粘贴 | 必需 |
| 有一个邮箱账号 | 必需 |
| 知道「命令行」是什么 | 加分项 |
| 会写代码 | ❌ 不需要 |

---

## 小白必读：这些是什么

在动手之前，先花 2 分钟弄清楚这几个东西是什么，后面会轻松很多。

### 比喻理解

想象你要去一家图书馆查资料：

| 概念 | 比喻 | 在本教程中 |
|------|------|-----------|
| **Claude Code** | 你的「私人秘书」 | 一个帮你写代码、改 bug 的终端工具 |
| **DeepSeek** | 秘书背后的「智囊团」 | 提供 AI 能力的公司 |
| **API Key** | 秘书的「工作证」 | 一串 `sk-...` 开头的密码，证明你有权限使用 |
| **Node.js** | 秘书工作的「办公室」 | 运行 Claude Code 所需的环境 |
| **settings.json** | 给秘书的「工作手册」 | 告诉 Claude Code 去哪找 DeepSeek |

### 一句话串起来

> 你在电脑上装好 Node.js（办公室），安装 Claude Code（雇秘书），然后配置 settings.json（给秘书一本工作手册，告诉它去找 DeepSeek 智囊团），最后用 API Key（工作证）证明身份——秘书就能帮你干活了。

---

## 第一步：注册 DeepSeek & 获取 API Key

> ⏱ 预计耗时：**5 分钟** &nbsp;&nbsp; 📊 难度：⭐（最简单）

### 1.1 访问注册页面

打开浏览器，访问 **[platform.deepseek.com](https://platform.deepseek.com)** → 点击右上角 **Sign Up**。

![DeepSeek 注册页面](https://raw.githubusercontent.com/KingandWorld/awesome-ai-resources/master/images/deepseek-signup.svg)

### 1.2 注册账号

- 填邮箱（QQ、163、Gmail 都行）
- 设置密码（**建议记在备忘录里**）
- 去邮箱点确认链接

### 1.3 创建 API Key

1. 登录后，左侧菜单找到 **API Keys**
2. 点击 **Create API Key**
3. 起个名字，比如 `claude-code`
4. 点击创建，**立刻复制**那串 `sk-...` 开头的字符

![API Keys 页面 — 点击 Create API Key](https://raw.githubusercontent.com/KingandWorld/awesome-ai-resources/master/images/deepseek-apikeys.svg)

> ⚠️ **这条 Key 只显示一次！** 关闭页面后就看不到了。请马上粘贴到记事本保存。

```
格式示例：sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```


### 💰 关于费用

| 项目 | 说明 |
|------|------|
| 付费模式 | 按量付费，用多少扣多少 |
| 月费/年费 | **没有** |
| 最低充值 | **¥1** |
| 价格水平 | 比 ChatGPT/GPT-4o 便宜 **几十倍** |

> ⚠️ **旧模型名即将废弃！** `deepseek-chat` 和 `deepseek-reasoner` 将于 **2026年7月24日** 下线，请改用：
> - `deepseek-chat` → `deepseek-v4-flash`
> - `deepseek-reasoner` → `deepseek-v4-pro`（thinking 模式）

**价格参考（来自 [DeepSeek 官方定价](https://api-docs.deepseek.com/quick_start/pricing)）：**

| 模型 | 输入 / 百万 tokens | 输出 / 百万 tokens | 约合人民币 |
|------|:-:|:-:|:-:|
| `deepseek-v4-flash` 轻量 | $0.14 | $0.28 | ≈ ¥1.0 / 百万 |
| `deepseek-v4-pro` 主力 | $0.435 | $0.87 | ≈ ¥3.2 / 百万 |

> 💡 直观感受：用 `deepseek-v4-pro` 写一整天代码，花费通常**不到几毛钱**。充 ¥1 能用很久。

### ✅ 第一步检查点

- [ ] 能登录 platform.deepseek.com
- [ ] 记事本里存好了 `sk-...` 开头的 API Key
- [ ] 知道充值的入口在哪

---

## 第二步：安装 Node.js 和 Git

> ⏱ 预计耗时：**10 分钟** &nbsp;&nbsp; 📊 难度：⭐（一直点 Next 就行）

### 2.1 安装 Node.js

<!-- TODO: 插入截图：nodejs.org 首页，圈出 LTS 按钮 -->

1. 访问 **[nodejs.org](https://nodejs.org)**
2. 点左边那个 **LTS** 按钮下载（推荐，最稳定）
3. 双击安装包，一路 **Next**
4. 遇到 License 勾选同意，路径不用改
5. 点 **Install** → 等进度条跑完 → **Finish**

### 2.2 Windows 用户：还要装 Git

> Mac 用户跳过这一步，Mac 自带 Git。

1. 访问 **[git-scm.com/download/win](https://git-scm.com/download/win)**
2. 下载后双击安装
3. 一路 **Next**（全部默认选项即可）
4. 点 **Finish**

### 2.3 验证安装

按 `Win + R`，输入 `cmd` 回车（Mac 用户打开终端），依次输入：

```bash
# 检查 Node.js
node --version
# 应该显示类似 v20.18.0 ✅

# 检查 Git
git --version
# 应该显示类似 git version 2.44.0 ✅
```

> ❌ 如果某条命令报「找不到」，说明那个软件没装好。常见原因：安装时没勾选"添加到 PATH"。最简单的办法：**卸载重装**，重装时注意看有没有 `Add to PATH` 的选项，确保勾选。
>
> 💡 **PowerShell 用户注意：** 如果你的默认终端是 PowerShell（Windows 11 默认），上面的 `%USERPROFILE%` 写法不生效，请改用 `$env:USERPROFILE`。建议按 `Win + R` 输入 `cmd` 回车，直接用经典命令行。

### ✅ 第二步检查点

- [ ] `node --version` 能正常输出
- [ ] `git --version` 能正常输出
- [ ] 命令行窗口保持打开（下一步继续用）

---

## 第三步：安装 Claude Code

> ⏱ 预计耗时：**3 分钟**（取决于网速） &nbsp;&nbsp; 📊 难度：⭐

### 3.1 执行安装

在命令行里输入：

```bash
npm install -g @anthropic-ai/claude-code
```

等待滚动文字跑完，看到类似输出表示成功：

```
added xxx packages in xx seconds
```

> 🐢 如果卡住很久不动，可能是网络问题。试试换国内镜像：
> ```bash
> npm config set registry https://registry.npmmirror.com
> ```
> 然后重新运行安装命令。

### 3.2 验证安装

```bash
claude --version
# 应该显示类似 claude-code 0.x.x ✅
```

> 💡 **不想全局安装？** 也可以用 `npx @anthropic-ai/claude-code` 直接运行，无需 `npm install -g` 步骤。

### ✅ 第三步检查点

- [ ] `claude --version` 正常输出版本号
- [ ] 没有报错

---

## 第四步：配置 Claude Code 使用 DeepSeek

> ⏱ 预计耗时：**5 分钟** &nbsp;&nbsp; 📊 难度：⭐⭐（需要认真复制粘贴）

### 原理速览

Claude Code 默认去找 Anthropic（Claude 母公司）的服务器。DeepSeek 提供了一套「和 Anthropic 格式一模一样」的接口。我们只需要修改配置文件，让它改去 DeepSeek 的地址。

```
默认：Claude Code → api.anthropic.com
改后：Claude Code → api.deepseek.com/anthropic
```

### 4.1 创建/打开配置文件

**Windows 用户**：在命令行输入
```bash
notepad %USERPROFILE%\.claude\settings.json
```

> 如果提示找不到文件：先创建 `.claude` 文件夹。
> 打开「我的电脑」→ 地址栏输入 `%USERPROFILE%` 回车 → 新建文件夹，命名为 `.claude`

**Mac 用户**：
```bash
mkdir -p ~/.claude
open -a TextEdit ~/.claude/settings.json
```

### 4.2 复制配置内容

把下面这段 **完整复制** 到打开的记事本里：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "这里填你的 DeepSeek API Key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}
```

<details>
<summary>📖 每个配置项是干什么的？（点击展开）</summary>

| 配置项 | 大白话解释 |
|--------|-----------|
| `ANTHROPIC_BASE_URL` | 「去这个地址找 AI」——指向 DeepSeek 服务器 |
| `ANTHROPIC_AUTH_TOKEN` | 「这是我的证件」——你的 API Key |
| `ANTHROPIC_MODEL` | 「默认用哪个大脑」——主力模型 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 处理**复杂任务**用的模型（比如大项目重构） |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 处理**日常任务**用的模型 |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 处理**简单任务**用的模型（快速且便宜） |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 禁用不必要联网（用第三方 API **必须开**，设为 `1`） |
| `CLAUDE_CODE_EFFORT_LEVEL` | AI 努力程度，`max` 效果最好 |
</details>

### 4.3 填入你的 API Key

把 `"这里填你的 DeepSeek API Key"` 替换为第一步保存的那串 `sk-...`。

比如：
```json
"ANTHROPIC_AUTH_TOKEN": "sk-a1b2c3d4e5f6g7h8i9j0...",
```

### 4.4 保存

按 `Ctrl + S` 保存，关闭记事本。

### ✅ 第四步检查点

- [ ] settings.json 文件存在且路径正确
- [ ] ANTHROPIC_AUTH_TOKEN 填的是你自己的 `sk-...` Key
- [ ] ANTHROPIC_BASE_URL 写的是 `https://api.deepseek.com/anthropic`（不是 `/v1`）

---

## 第五步：启动 & 验证

> ⏱ 预计耗时：**3 分钟** &nbsp;&nbsp; 📊 难度：⭐

### 5.1 进入工作目录

在命令行里进入你准备写代码的文件夹（或者新建一个测试文件夹）：

**Windows**：
```bash
cd %USERPROFILE%\Desktop
mkdir my-code && cd my-code
```

**Mac**：
```bash
cd ~/Desktop
mkdir my-code && cd my-code
```

### 5.2 启动 Claude Code

```bash
claude
```

首次启动会弹出用户协议，输入 `y` 回车同意。

### 5.3 验证配置

看到提示符变成 `▶` 后，先检查配置：

```
/status
```

确认：
- **API Endpoint** 显示 `https://api.deepseek.com/anthropic` ✅
- **Model** 显示 `deepseek-v4-pro` ✅

![Claude Code /status 输出 — 确认连接 DeepSeek](https://raw.githubusercontent.com/KingandWorld/awesome-ai-resources/master/images/claude-status.svg)

### 5.4 试跑一个任务

在 `▶` 后面输入：

```
写一个简单的网页，显示 "Hello World"
```

如果它开始思考并生成代码，恭喜，一切就绪！🎉

### ✅ 第五步检查点

- [ ] `/status` 确认用的是 DeepSeek
- [ ] 成功让 AI 干了一件事（哪怕只是输出 Hello World）

---

## 常见问题 FAQ

### 🔍 快速排错流程

遇到问题别着急，按这个顺序查：

```
出错了？
  ├─ claude 命令找不到？ → 重开命令行，或用 npx
  ├─ npm install 很慢？  → 换国内镜像（见下方 FAQ）
  ├─ API 连不上？
  │   ├─ /status 检查 URL 是不是 api.deepseek.com
  │   ├─ API Key 填对了没有？
  │   └─ 账户余额 > 0？
  └─ 还不行？ → 看下面的具体 FAQ
```

<details open>
<summary><b>Q：Node.js 装不上 / 报错？</b></summary>

1. 确认系统是 64 位（设置 → 系统 → 关于 → 系统类型）
2. 去 nodejs.org 下载 **Windows 64-bit** 版本
3. 安装时确保勾选了 **Add to PATH**
4. 重启电脑再试
</details>

<details>
<summary><b>Q：npm install 巨慢或卡死？</b></summary>

网络问题，换国内镜像：
```bash
npm config set registry https://registry.npmmirror.com
npm install -g @anthropic-ai/claude-code
```
</details>

<details>
<summary><b>Q：git 连不上 GitHub（SSL 错误 / 超时）？</b></summary>

国内网络访问 GitHub 不稳定，试试以下方法：

<details>
<summary>🔧 方法一：取消 SSL 验证（临时解决）</summary>

```bash
git config --global http.sslVerify false
```
> ⚠️ 仅建议临时用，用完记得设回 `true`。
</details>

<details>
<summary>🔧 方法二：配置代理（推荐）</summary>

```bash
# 如果使用 Clash/V2Ray 等代理工具（默认 7890 端口）
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```
</details>
</details>

<details>
<summary><b>Q：claude 命令提示「找不到」？</b></summary>

- 关掉命令行，重新打开再试
- 或者直接运行：`npx @anthropic-ai/claude-code`
</details>

<details>
<summary><b>Q：启动后 API 连接失败 / 报错？</b></summary>

按顺序排查：
1. `/status` 确认 Base URL 是 `https://api.deepseek.com/anthropic`
2. 确认 API Key 完整复制了（`sk-...` 开头）
3. 确认 DeepSeek 平台账户余额 > 0
4. 浏览器访问 `api.deepseek.com` 测试网络连通性
</details>

<details>
<summary><b>Q：怎么确认用的是 DeepSeek 不是 Claude？</b></summary>

输入 `/status`，看 **API Endpoint**。如果是 `api.deepseek.com` 就对了。
</details>

<details>
<summary><b>Q：API Key 不小心泄露了？</b></summary>

登录 DeepSeek 平台 → API Keys → 删掉泄露的 Key → 创建新 Key → 更新 settings.json 里的 `ANTHROPIC_AUTH_TOKEN`。
</details>

<details>
<summary><b>Q：deepseek-chat 还能用吗？</b></summary>

`deepseek-chat` 和 `deepseek-reasoner` 将于 **2026年7月24日** 下线。尽快改用 `deepseek-v4-pro` 和 `deepseek-v4-flash`。
</details>

<details>
<summary><b>Q：不想用了怎么卸载？</b></summary>

```bash
npm uninstall -g @anthropic-ai/claude-code
```
配置文件 `%USERPROFILE%\.claude\settings.json` 需要手动删除。
</details>

---

## 进阶技巧

### 切换模型

修改 `settings.json` 中 `ANTHROPIC_MODEL` 的值：

| 场景 | 推荐模型 |
|------|---------|
| 主力干活 | `deepseek-v4-pro` |
| 简单快速任务 | `deepseek-v4-flash` |

### 配合 VS Code 使用

安装 [Claude Code VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)，它会自动读取 `settings.json` 的配置，无需重复设置。

### 项目级配置

如果你不同项目想用不同的配置（比如不同项目用不同模型），可以在项目根目录创建 `.claude/settings.local.json`：

```bash
mkdir -p .claude
```

写入和全局 `settings.json` 同样的格式。项目级配置会**覆盖**全局配置，只影响当前项目。

### 查看当前状态

在 Claude Code 里随时输入 `/status`：

- API Endpoint：当前连接的服务器
- Model：当前使用的模型
- Token Usage：已消耗的 token 数

### 常用命令速查

| 命令 | 作用 |
|------|------|
| `/help` | 查看所有可用命令 |
| `/status` | 查看当前配置和用量 |
| `/clear` | 清空当前对话 |
| `/cost` | 查看 token 消耗 |
| `exit`或`Ctrl+C` | 退出 Claude Code |

---

## 名词解释

| 术语 | 大白话解释 |
|------|-----------|
| **API** | 程序之间通信的接口。本教程里，Claude Code 通过 API 和 DeepSeek 服务器对话 |
| **API Key** | 一串密码，证明你有权限使用 API |
| **Token** | AI 处理文本的最小单位。一个中文字 ≈ 1.5 token，一个英文词 ≈ 1 token |
| **Node.js** | 一个让 JavaScript 能在命令行运行的环境 |
| **npm** | Node.js 的「应用商店」，用来下载安装各种工具 |
| **settings.json** | Claude Code 的配置文件，JSON 格式 |
| **JSON** | 一种数据格式，由 `{ }` `[ ]` `" "` 组成 |
| **LTS** | Long Term Support，长期支持版，最稳定 |
| **终端/命令行** | 那个黑色背景、白色字的窗口 |

---

## 📌 速查卡片

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│  ① 注册 DeepSeek 账号 → 创建 API Key (sk-...)            │
│                                                          │
│  ② 安装 Node.js + Git (Windows)                          │
│                                                          │
│  ③ npm install -g @anthropic-ai/claude-code               │
│                                                          │
│  ④ 编辑 ~/.claude/settings.json → 填入 DeepSeek 配置      │
│                                                          │
│  ⑤ 终端输入 claude → /status 验证 → 开始使用！            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 参考链接

- [DeepSeek 官方文档 - Claude Code 集成](https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code)
- [DeepSeek 官方定价](https://api-docs.deepseek.com/quick_start/pricing)
- [DeepSeek 平台 - API Keys 管理](https://platform.deepseek.com/api_keys)
- [Node.js 官网](https://nodejs.org)
- [Git for Windows](https://git-scm.com/download/win)
- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code)

---

## 🤝 贡献

如果发现文档有误或过时，欢迎提 Issue 或 PR。

---

<p align="center">
  <b>觉得有用？给个 ⭐ Star 支持一下</b><br/>
  <sub>有问题欢迎在 Issues 提问</sub>
</p>
