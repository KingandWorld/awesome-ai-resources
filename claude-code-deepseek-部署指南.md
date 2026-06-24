# 🤖 Claude Code + DeepSeek API 部署指南

> 写给小白朋友的零基础教程，一步步带你装好。

---

## 📋 你需要准备什么

| 项目 | 说明 |
|------|------|
| 💻 **一台电脑** | Windows / Mac 都可以 |
| 🌐 **能上网** | 全程需要联网下载东西 |
| 💰 **一点点钱** | DeepSeek API 首次注册送 500 万 tokens，够用很久 |
| ⏱ **约 30 分钟** | 跟着做一遍就装好了 |

---

## 第一步：注册 DeepSeek 账号 & 获取 API Key

DeepSeek API Key 相当于你的「钥匙」，Claude Code 用它来调用 DeepSeek 的 AI 能力。

### 1.1 打开注册页面

1. 打开浏览器，访问 **[https://platform.deepseek.com](https://platform.deepseek.com)**（DeepSeek 官方平台）
2. 点击右上角的 **「Sign Up」**（注册）

### 1.2 填写注册信息

- **邮箱**：填你的邮箱（QQ 邮箱、163 邮箱都行）
- **密码**：设置一个密码（建议记在备忘录里）
- 收到验证邮件后，点击链接确认

### 1.3 获取 API Key

1. 登录成功后，页面左侧找到 **「API Keys」**（API 密钥）
2. 点击 **「Create API Key」**（创建密钥）
3. 给你的 Key 起个名字，比如 `claude-code`
4. 复制出来的那串 **sk-... 开头的字符**（它就是你的 API Key）

> ⚠️ **重要**：这串 Key 只显示一次，请立刻粘贴到记事本里保存好！
> 格式大概是：`sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

📌 **关于费用**：DeepSeek 注册即送 **500 万 tokens**（大约可以问几千次问题），用完之后才需要充值，量小的话几乎不花钱。

---

## 第二步：安装 Node.js

Claude Code 需要 Node.js 才能运行。Node.js 可以理解为一个「运行环境」。

### 2.1 下载 Node.js

1. 打开浏览器，访问 **[https://nodejs.org](https://nodejs.org)**
2. 你会看到两个下载按钮：
   - **LTS**（左边那个，推荐）→ 点击下载
3. 等待下载完成

### 2.2 安装 Node.js

1. 双击下载好的安装包（文件名类似 `node-v20.x.x-x64.msi` 或 `.pkg`）
2. 一路点 **「Next」（下一步）**
3. 遇到 **「License Agreement」** 勾选同意
4. 安装路径默认就行，不用改
5. 点 **「Install」（安装）**
6. 安装完成后点 **「Finish」**

### 2.3 验证安装是否成功

1. 按键盘上的 **`Win + R`**，输入 `cmd`，回车（打开命令行）
2. 在黑窗口里输入以下命令，然后回车：

```
node --version
```

3. 如果看到类似 `v20.18.0` 的字样，说明 Node.js 安装成功！✅

> ❓ Mac 用户：打开「启动台」→「其他」→「终端」

---

## 第三步：安装 Claude Code

### 3.1 在命令行中安装

1. 在刚才的黑窗口（终端）里，输入以下命令，然后回车：

```
npm install -g @anthropic-ai/claude-code
```

2. 等待它下载安装，期间会有一些滚动的文字，**不需要管它**
3. 看到类似这样的提示就是安装成功了：

```
added xxx packages in xx seconds
```

### 3.2 验证安装

执行以下命令：

```
claude --version
```

如果显示版本号（比如 `claude-code 0.x.x`），说明安装成功！✅

---

## 第四步：配置 Claude Code 使用 DeepSeek API

### 4.1 打开配置目录

在命令行里输入以下命令，然后回车：

```
notepad %USERPROFILE%\.claude\settings.json
```

> ❓ **如果报错说找不到路径**：
> 1. 先手动创建一个文件夹：打开「我的电脑」，在地址栏输入 `%USERPROFILE%` 回车
> 2. 在里面新建一个文件夹，名字叫 `.claude`（注意前面有个点）
> 3. 然后再运行上面的命令

### 4.2 写入配置

把下面的内容**完整复制**进去：

```json
{
  "model": "deepseek-chat",
  "apiKey": "这里填你的 DeepSeek API Key",
  "modelSettings": {
    "deepseek-chat": {
      "provider": "deepseek",
      "baseUrl": "https://api.deepseek.com/v1"
    }
  }
}
```

### 4.3 替换 API Key

把 `"这里填你的 DeepSeek API Key"` 替换成你在第一步复制的那个 `sk-...` 开头的 Key。

**保存文件**（按 `Ctrl + S`），然后关闭记事本。

---

## 第五步：开始使用！

### 5.1 随便找一个文件夹

在电脑上新建一个文件夹（比如在桌面上新建一个 `my-code`），然后用命令行进入它：

```
cd 桌面\my-code
```

### 5.2 启动 Claude Code

在命令行输入：

```
claude
```

第一次启动会显示一些协议，输入 `y` 回车同意即可。

### 5.3 测试一下

看到提示符变成 `▶` 说明启动成功了！试着输入：

```
写一个简单的网页，显示 hello world
```

如果 Claude Code 开始思考并给出答案，说明一切配置成功！🎉

> 输入 `/help` 可以查看所有命令
> 输入 `/clear` 可以清空对话
> 输入 `exit` 或按 `Ctrl+C` 可以退出

---

## ❓ 常见问题

### Q：安装 Node.js 时报错怎么办？
先确认你的电脑系统是 64 位的（大部分电脑都是）。下载时选 Windows 64-bit 版本。还是不行的话，**重启电脑**再试一次。

### Q：`npm install` 很慢或卡住了？
网络问题。可以换成国内镜像加速：

```
npm config set registry https://registry.npmmirror.com
```

然后重新执行安装命令。

### Q：`claude` 命令提示「找不到」？
关掉命令行重新打开一次，或者试试：

```
npx @anthropic-ai/claude-code
```

### Q：API Key 不小心泄露了怎么办？
登录 DeepSeek 平台 → API Keys → 点那个 Key 右边的「删除」→ 重新生成一个新的。

---

## 📌 快速回顾

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  ① 注册 DeepSeek → 拿到 API Key (sk-...)            │
│                                                     │
│  ② 安装 Node.js (官网下载，一路 Next)                │
│                                                     │
│  ③ npm install -g @anthropic-ai/claude-code          │
│                                                     │
│  ④ 配置 settings.json (填上你的 API Key)             │
│                                                     │
│  ⑤ 运行 claude → 开始玩！                           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 🚀 进阶技巧（以后再看）

- **贴代码分享**：Claude Code 可以直接读取你文件夹里的代码文件，帮你修改、找 bug
- **配合 VS Code**：如果你会用 VS Code，安装 Claude Code 扩展体验更好（直接在编辑器里用）
- **换模型**：把配置里的 `deepseek-chat` 换成其他 DeepSeek 模型名就行（比如 `deepseek-reasoner`）

---

> 有任何问题，随时把报错截图发给朋友帮忙看看！
