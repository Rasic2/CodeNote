# Docker 教程

[Docker](https://www.docker.com/) 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。它可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

## Docker 常用命令

- 从镜像仓库中拉取镜像（IMAGE）

```bash
docker pull IMAGE
```

- 以某个镜像（IMAGE）启动一个容器，并在容器内执行 bash 命令

```bash
docker run -it IMAGE "/bin/bash"
```

- 列出运行中的容器

```bash
docker ps
```

加上`-a` 参数列出所有容器（包括未运行的）

- 启动一个容器

```bash
docker start container_id
```

- 进入一个容器

```bash
docker attach container_id
```

- 复制本地文件到镜像

```
docker cp local_path container_id:docker_path
```

- 复制镜像文件到本地

```
docker cp container_id:docker_path local_path
```

- 退出 docker 终端（快捷键`Ctrl+P+Q`）

- 终止一个容器

```bash
docker stop container_id
```
