# ComfyUI_Bench：部署与开发环境记录

## 1. Docker 拉取问题

之前讨论过一个现象：

```text
在别的目录打开终端，docker pull 可以成功；
在项目目录的终端里 pull 不成功。
```

这通常说明问题不是 Docker 本身，而是当前 shell 环境、代理环境变量、项目目录中的配置、终端会话变量等导致的。

排查命令：

```bash
env | grep -i proxy
docker info
curl --noproxy '*' -I --connect-timeout 10 https://docker.m.daocloud.io/v2/
```

## 2. 如果镜像已经拉到本地，还需要在项目终端重新 pull 吗

一般不需要。

Docker 镜像是全局保存在本机 Docker Engine 里的，不是按项目目录保存。

如果本地已经存在镜像：

```bash
docker images
```

能看到目标镜像，就可以在任何项目里使用。

## 3. 项目开发环境建议

开发 ComfyUI_Bench 时，需要关注：

- Python 虚拟环境。
- FastAPI 服务。
- Redis 服务。
- SQL 数据库。
- Docker / docker compose。
- ComfyUI Runtime。
- 前端开发服务器。

## 4. 推荐本地启动顺序

```text
1. 启动数据库
2. 启动 Redis
3. 启动后端 FastAPI
4. 启动 Worker
5. 启动 ComfyUI
6. 启动前端
```

## 5. 后续需要整理

- docker-compose.yml。
- .env.example。
- 本地启动命令。
- 后端端口。
- 前端端口。
- Redis 连接配置。
- 数据库迁移命令。

