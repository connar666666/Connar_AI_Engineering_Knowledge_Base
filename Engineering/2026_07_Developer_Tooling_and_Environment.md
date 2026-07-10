# Engineering：2026 年 7 月工具与开发环境增量

## 1. Docker 镜像与镜像仓库

### 镜像

Docker 镜像是应用运行环境的只读模板，通常包含：

- 基础操作系统文件。
- 运行时。
- 依赖。
- 应用代码。
- 默认启动命令。

```text
Dockerfile
  ↓ build
Image
  ↓ run
Container
```

### 镜像仓库

镜像仓库用于存储和分发镜像，例如 Docker Hub、GitHub Container Registry、阿里云容器镜像服务。

```text
本地 build / pull
  ↕
Registry
  ↕
开发机 / 测试环境 / 生产服务器
```

阿里云镜像仓库可能有两种含义：

1. 镜像加速地址：代理拉取公共镜像。
2. 企业私有仓库：保存团队自己的镜像。

两者不要混淆。

## 2. 镜像不是按项目目录保存

Docker 镜像由 Docker Engine 全局管理，不属于当前终端目录。

如果在另一个目录执行 `docker pull` 成功：

```bash
docker images
```

能够看到镜像后，当前项目的 Compose 或 Docker 命令通常可以直接使用，不需要在项目终端重复拉取。

如果只有某个项目终端失败，优先检查：

- 当前 shell 的代理环境变量。
- 项目级 `.env`。
- Compose 配置中的 registry 地址。
- 不同终端是否连接同一个 Docker Context。
- Shell 启动脚本是否覆盖变量。

## 3. Git commit SHA 能否修改

Commit SHA 是根据提交内容、父提交、作者、时间和提交信息等计算得到的标识，不能对一个已经存在的提交“原地改 ID”。

执行以下操作会创建新提交并产生新 SHA：

- `git commit --amend`。
- `git rebase -i`。
- 修改历史提交内容或 message。
- 改变父提交关系。

如果历史已经推送，需要强制推送：

```bash
git push --force-with-lease
```

这会影响其他协作者，应谨慎使用。优先 `--force-with-lease`，不要直接使用无保护的 `--force`。

## 4. PR 中的 Test、CI 和 Lint

```text
Lint：静态检查代码质量和风格
Test：运行代码验证行为
CI：自动执行 lint、test、build 等检查的流水线
```

一个常见流水线：

```text
checkout
  ↓
install dependencies
  ↓
lint
  ↓
type check
  ↓
unit tests
  ↓
integration tests
  ↓
build
```

检查通过不代表架构一定合理，仍然需要人工 Review。

## 5. Codex 插件安装后不可用

当插件列表显示已安装，但命令或能力不可调用时，排查顺序：

1. 确认插件安装到当前正在运行的 Codex 环境，而不是另一个 Node/npm 前缀。
2. 检查插件目录、manifest 和必要的 `SKILL.md` / 配置文件。
3. 检查 YAML frontmatter 是否完整。
4. 退出并重新启动当前 Codex CLI 会话。
5. 确认工作区信任和插件权限。
6. 检查版本兼容性。
7. 查看启动日志中是否有加载失败。

所谓 reload 通常不是刷新一个静态页面，而是让宿主进程重新扫描插件。CLI 中最可靠的方式一般是完全退出并重新启动；如果产品提供明确 reload 命令，再优先使用官方命令。

## 6. WSL 中 Node/npm 混用

典型错误：在 WSL 中执行 `codex`，实际调用到 Windows 路径：

```text
/mnt/c/Users/.../AppData/Roaming/npm/...
```

然后缺少 Linux 平台依赖。

目标是让 WSL 使用 Linux 自己的 Node、npm 和全局安装目录：

```bash
which node
which npm
which codex
npm config get prefix
type -a node npm codex
```

如果路径指向 `/mnt/c/...`，说明 Windows PATH 被注入 WSL 或旧 alias / shim 仍然存在。

推荐：

- 在 WSL 内安装 Linux Node。
- 使用 nvm 或用户级 npm prefix。
- 清理指向 Windows npm 的 alias 和 PATH 优先级。
- 重新打开 shell 并执行 `hash -r`。
- 在 WSL 内重新安装对应 CLI。

不要用 Windows 安装目录中的平台包在 Linux 环境运行。

## 7. WSL 代理排查

WSL 中的 `127.0.0.1` 指向 WSL 自身，不一定是 Windows 主机。

排查：

```bash
ip route | grep default
cat /etc/resolv.conf
nc -vz <windows-host-ip> <proxy-port>
curl -x http://<windows-host-ip>:<port> -I https://github.com
```

如果连接被拒绝：

- Windows 代理程序可能只监听本机回环地址。
- 未开启允许局域网连接。
- 端口错误。
- Windows 防火墙阻止 WSL。
- TUN 模式和手动代理模式配置冲突。

## 8. LaTeX Algorithm 包冲突

同一文档中重复加载：

```latex
\usepackage[noend]{algpseudocode}
```

或同时混用互不兼容的 algorithm 包，可能导致命令重定义、环境不显示或排版异常。

推荐最小组合：

```latex
\usepackage{algorithm}
\usepackage[noend]{algpseudocode}
\usepackage{amsmath,amssymb}
\usepackage{float}
```

原则：

- 同一个包只加载一次。
- 不同时使用多个提供同名环境的算法包。
- 先用最小可编译示例确认环境。
- 再逐个加入 `ctex`、caption、geometry 等其他包。

## 9. MacBook Air 与 Pro 的工程选择原则

对于主要工作是浏览器、VS Code、终端、远程开发和轻量代码运行：

- Air 的性能通常已经足够。
- Pro 的主要优势是持续高负载散热、屏幕、接口和更高配置上限。
- 本地长期跑 Docker、多服务、视频渲染或模型时，内存和散热比型号名称更重要。

选购时优先考虑：

```text
内存容量 > 存储是否够用 > 芯片代际 > Air/Pro 标签
```

二手设备还要检查电池循环、屏幕、键盘、主板维修、激活锁和序列号状态。具体价格具有时效性，不应长期固化在知识库结论中。

## 10. 工程排障原则

遇到“同一命令在不同目录或终端结果不同”时，不要先重装全部工具。按层检查：

```text
命令实际路径
  ↓
环境变量
  ↓
配置文件
  ↓
网络与代理
  ↓
运行时版本
  ↓
项目级依赖
  ↓
宿主进程日志
```

先确定差异发生在哪一层，再修改对应配置。