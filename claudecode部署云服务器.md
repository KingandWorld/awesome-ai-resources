# Claude Code 部署云服务器 完整指南

> 适用环境：腾讯云轻量应用服务器 2核2G（也适用于其他云服务器）

---

## 前置说明

**Claude Code 本身是轻量级 CLI 客户端**，AI 推理在 Anthropic 服务器上完成。你的云服务器只需要运行 CLI + 基本的开发工具，2核2G 足够应对个人项目和小型开发任务。

---

## 第一步：服务器基础配置

### 1.1 SSH 登录服务器

```bash
ssh root@你的服务器IP
```

### 1.2 更新系统并安装基础工具

```bash
# 更新包管理器
apt update && apt upgrade -y   # Ubuntu/Debian
# 或
yum update -y                  # CentOS

# 安装基础工具
apt install -y curl wget git build-essential unzip
```

### 1.3 创建普通用户（推荐，不要用 root 日常操作）

```bash
# 创建用户
adduser devuser

# 加入 sudo 组
usermod -aG sudo devuser

# 切换到新用户
su - devuser
```

### 1.4 配置 Swap（2GB 内存建议开启，防内存溢出）

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 设置开机自动挂载
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# 验证
sudo swapon --show
```

---

## 第二步：安装 Node.js

Claude Code 需要 Node.js 18+ 环境。

### 2.1 使用 NodeSource 安装（推荐方法一）

```bash
# 安装 Node.js 22.x（LTS）
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# 验证版本
node -v   # 应显示 v22.x.x
npm -v    # 应显示 10.x.x
```

### 2.2 使用 nvm 安装（推荐方法二，可切换版本）

```bash
# 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

# 重新加载 shell 配置
source ~/.bashrc

# 安装最新的 LTS 版本
nvm install 22
nvm use 22
nvm alias default 22

# 验证
node -v
npm -v
```

### 2.3 配置 npm 全局安装路径（可选）

```bash
# 创建全局安装目录
mkdir -p ~/.npm-global

# 配置 npm
npm config set prefix '~/.npm-global'

# 添加到 PATH
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## 第三步：安装 Claude Code

### 3.1 全局安装

```bash
npm install -g @anthropic-ai/claude-code

# 验证安装
claude --version
```

### 3.2 首次启动与认证

```bash
# 启动 Claude Code
claude
```

首次启动时会提示认证，有两种方式：

**方式一：API Key 认证**

```bash
# 在 https://console.anthropic.com/ 创建 API Key
# 设置环境变量
export ANTHROPIC_API_KEY="sk-ant-api03-你的API密钥"

# 写入 .bashrc 永久生效
echo 'export ANTHROPIC_API_KEY="sk-ant-api03-你的API密钥"' >> ~/.bashrc
source ~/.bashrc
```

**方式二：OAuth 登录**

```bash
# 启动后会自动打开浏览器（服务器上需要用 --no-browser）
claude login --no-browser
# 按提示在本地浏览器打开 URL，输入验证码完成认证
```

---

## 第四步：保持 Claude Code 后台运行

云服务器断开 SSH 后，前台进程会被杀掉。需要用工具保持 Claude Code 持续运行。

### 4.1 使用 tmux（推荐）

```bash
# 安装 tmux
sudo apt install -y tmux

# 创建新会话
tmux new -s claude

# 在 tmux 中启动 Claude Code
claude

# 断开 tmux（Claude Code 继续后台运行）
# 快捷键：Ctrl+B 然后按 D

# 重新连接回 tmux 会话
tmux attach -t claude

# 查看所有会话
tmux ls

# 彻底关闭会话
tmux kill-session -t claude
```

### 4.2 tmux 常用快捷键

| 快捷键 | 功能 |
|---|---|
| `Ctrl+B` + `D` | 断开会话（后台运行） |
| `Ctrl+B` + `[` | 进入滚动模式（可用方向键翻页） |
| `Ctrl+B` + `C` | 新建窗口 |
| `Ctrl+B` + `N` | 下一个窗口 |
| `Ctrl+B` + `P` | 上一个窗口 |
| `Ctrl+B` + `%` | 左右分屏 |
| `Ctrl+B` + `"` | 上下分屏 |

### 4.3 使用 screen（备选方案）

```bash
# 安装 screen
sudo apt install -y screen

# 创建会话
screen -S claude

# 在 screen 中启动 Claude Code
claude

# 断开会话
# 快捷键：Ctrl+A 然后按 D

# 重新连接
screen -r claude
```

---

## 第五步：非交互式使用（CI/脚本场景）

### 5.1 单次任务模式

```bash
# 用管道传入任务，--print 输出结果后退出
echo "审查 src/ 目录下的代码，找出所有潜在的安全问题" | claude --print

# 从文件读任务
claude --print < 任务描述.txt

# 一行命令
claude --print -p "帮我给 src/utils.ts 写单元测试"
```

### 5.2 在 CI/CD 中使用

**GitHub Actions 示例：**

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '22'
      - run: npm install -g @anthropic-ai/claude-code
      - run: |
          echo "Review this PR diff. Focus on bugs, security issues, and code quality." | \
          claude --print --model claude-sonnet-4-6
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### 5.3 定时任务（Cron）

```bash
# 编辑 crontab
crontab -e

# 每天凌晨 2 点执行代码分析
0 2 * * * cd /home/devuser/myproject && echo "分析今天的所有变更，生成日报" | claude --print >> /var/log/claude-daily.log 2>&1
```

---

## 第六步：内存优化技巧

2GB 内存下运行 Claude Code，下列技巧能有效避免 OOM：

### 6.1 控制 Node.js 内存上限

```bash
# 限制 Node.js 最大堆内存为 512MB
export NODE_OPTIONS="--max-old-space-size=512"

# 写入 .bashrc
echo 'export NODE_OPTIONS="--max-old-space-size=512"' >> ~/.bashrc
```

### 6.2 减小 Git 内存占用

```bash
# 浅克隆大型仓库
git clone --depth=1 https://github.com/某大型仓库.git

# 减少 Git 的 pack 内存
git config --global pack.windowMemory "100m"
git config --global pack.packSizeLimit "100m"
git config --global core.packedGitWindowSize "16m"
git config --global core.packedGitLimit "32m"
```

### 6.3 监控内存使用

```bash
# 实时查看内存
watch -n 2 free -h

# 查看内存消耗最大的进程
ps aux --sort=-%mem | head -10

# 安装 htop 可视化监控
sudo apt install -y htop
```

### 6.4 释放缓存（紧急情况）

```bash
# 清理系统缓存（不会影响应用数据）
sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches

# 清理 apt 缓存
sudo apt clean
sudo apt autoremove -y
```

---

## 第七步：项目初始化

### 7.1 从 GitHub 拉取项目

```bash
# 生成 SSH Key（如果没有）
ssh-keygen -t ed25519 -C "your-email@example.com"

# 复制公钥，添加到 GitHub SSH Keys
cat ~/.ssh/id_ed25519.pub

# 克隆项目
git clone git@github.com:你的用户名/你的仓库.git
cd 你的仓库
```

### 7.2 启动 Claude Code 开始工作

```bash
# 切换到项目目录
cd ~/myproject

# 在 tmux 中启动 Claude Code
tmux new -s claude

# 启动 claude
claude

# 在 Claude Code 交互界面中，直接描述你的任务
# 例如："帮我分析这个项目的结构，然后实现 XXX 功能"
```

---

## 第八步：安全建议

### 8.1 防火墙配置

```bash
# 腾讯云轻量服务器默认有安全组，在控制台配置即可
# 服务器层面的防火墙：
sudo ufw allow 22/tcp       # 只开放 SSH
sudo ufw enable
sudo ufw status
```

### 8.2 API Key 安全

```bash
# 不要把 API Key 写在代码里
# 只用环境变量
# .bashrc 中设置的文件权限：
chmod 600 ~/.bashrc

# 定期在 https://console.anthropic.com/ 轮换 API Key
```

### 8.3 自动备份

```bash
# 每天备份工作目录到 /backup
echo '0 3 * * * tar -czf /backup/project-$(date +\%Y\%m\%d).tar.gz /home/devuser/myproject' | crontab -
```

---

## 常见问题排查

### Q: `claude: command not found`
```bash
# 确认全局安装路径在 PATH 中
npm list -g --depth=0
echo $PATH
# 需要时重新添加
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Q: 内存不足 (OOM Killer)
```bash
# 查看是否是 OOM Kill
dmesg | grep -i "out of memory"

# 增加 swap
sudo fallocate -l 4G /swapfile2
sudo chmod 600 /swapfile2
sudo mkswap /swapfile2
sudo swapon /swapfile2
```

### Q: npm 安装报权限错误
```bash
# 不要用 sudo npm install -g，先配置全局目录
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
# 然后再安装
npm install -g @anthropic-ai/claude-code
```

### Q: SSH 断开后 Claude Code 丢失
```bash
# 确认是否用了 tmux 或 screen
# 直接 SSH 登录运行的进程会在断开时被 kill
# 必须在 tmux 内运行
tmux new -s claude
claude
# Ctrl+B, D 断开会话
```

### Q: `claude` 启动很慢
```bash
# 首次启动需要下载依赖，耐心等待
# 后续启动会快很多
# 也可以先做一次预热：
echo "hello" | claude --print
```

---

## 附录：一键安装脚本

将以下内容保存为 `install-claude-code.sh`：

```bash
#!/bin/bash
set -e

echo "=== Claude Code 云服务器一键安装脚本 ==="

# 1. 基础依赖
echo "[1/5] 安装基础依赖..."
sudo apt update -y
sudo apt install -y curl git tmux build-essential

# 2. Swap
echo "[2/5] 配置 Swap..."
if [ ! -f /swapfile ]; then
    sudo fallocate -l 2G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
    echo "Swap 已创建"
else
    echo "Swap 已存在，跳过"
fi

# 3. Node.js
echo "[3/5] 安装 Node.js..."
if ! command -v node &> /dev/null; then
    curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
    sudo apt install -y nodejs
    echo "Node.js $(node -v) 安装完成"
else
    echo "Node.js $(node -v) 已安装"
fi

# 4. Claude Code
echo "[4/5] 安装 Claude Code..."
npm install -g @anthropic-ai/claude-code

# 5. 环境变量
echo "[5/5] 配置环境..."
echo 'export NODE_OPTIONS="--max-old-space-size=512"' >> ~/.bashrc
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc

echo ""
echo "=== 安装完成 ==="
echo "下一步："
echo "1. 设置 API Key: echo 'export ANTHROPIC_API_KEY=\"你的密钥\"' >> ~/.bashrc && source ~/.bashrc"
echo "2. 启动 tmux: tmux new -s claude"
echo "3. 启动 Claude Code: claude"
echo ""
```

使用方法：

```bash
chmod +x install-claude-code.sh
./install-claude-code.sh
```

---

> **最后提醒：** Claude Code 在你的服务器上是一个轻量级客户端，AI 推理在 Anthropic 云端完成。2核2G 跑个人项目足够了，但别把它当成完整开发工作站——大型编译、多容器编排等任务需要更高配置。
