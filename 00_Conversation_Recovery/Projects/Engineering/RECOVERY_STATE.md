# 工程环境与工具对话恢复状态

## 项目目标
持续记录 Mac、Windows、WSL、Docker、Git、Codex、Claude Code 和网络代理相关的排障经验。

## 对话历史关键点
- **用户环境**：同时使用 MacBook、Windows 和 WSL，多个运行时和代理配置容易互相影响。
- **用户遇到**：WSL 中调用到 Windows npm / Codex 路径，导致缺少 Linux 平台依赖。
- **用户遇到**：Docker 在不同终端或目录中表现不同。
- **助手补充**：排障应先确认实际命令路径、环境变量、运行时版本、网络、项目配置和宿主日志，而不是直接重装。
- **共同结论**：同一命令在不同终端结果不同，通常意味着环境层差异，不代表镜像或代码必须重复安装。

## 当前共同结论
- WSL 应使用 Linux 自己的 Node、npm 和 CLI 安装。
- Docker 镜像由 Docker Engine 全局管理，不属于某个项目目录。
- Git commit SHA 不能原地修改，重写提交会生成新 SHA。
- Codex 插件可见但不可调用时，要检查安装前缀、manifest、Skill 文件、版本和加载日志。
- 代理问题要区分 Windows 主机、WSL 地址、监听端口和是否允许局域网连接。

## 未完成事项
- 完全清理 WSL 中 Windows / Linux Node/npm 路径混用。
- 确认 Codex 插件真实加载目录和失败日志。
- 对 Docker 目录差异继续比较 `.env`、代理变量和 Docker Context。
- 将新的已验证命令持续更新到工程知识库。

## 知识库加载清单

```text
Engineering/Codex_Claude_Skills_and_Git.md
Engineering/2026_07_Developer_Tooling_and_Environment.md
Engineering/Windows_Hardware_and_Memory_Upgrade.md
02_ComfyUI_Bench/Deployment_and_DevOps.md
```

## 恢复后继续位置
用户提供命令输出时，先判断实际执行路径和环境层，再给出最小修改步骤；不要反复要求重装全部环境。