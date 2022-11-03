# Apptainer 教程

[Apptainer（原名 Singularity）](https://github.com/apptainer/apptainer)，已成为高性能计算（HPC）中使用最广泛的容器部署系统之一。该容器系统旨在以裸金属性能执行应用程序，同时保持高度的安全性、可移植性和再现性。

Apptainer 是一个开源项目，社区不断壮大，用户基础不断扩大。功能集如下所示：

- 支持信任的公钥/私钥签名。

- 与 Docker 和开放式容器倡议的兼容性。

- 加密。

- 可移植性。

- 容器无根运行以禁止权限提升。

- 利用 GPU、FPGA、高速网络和文件系统。

- 易于使用。

- 提供商业支持。

## Ubuntu 安装

```bash
> sudo apt-get update
> sudo apt-get install -y wget
> cd /tmp
> wget https://github.com/apptainer/apptainer/releases/download/v1.1.2/apptainer_1.1.2_amd64.deb
> sudo apt-get install -y ./apptainer_1.1.2_amd64.deb
```

## 语法

- 将公共镜像转为 sif

```bash
apptainer pull docker://ubuntu
```

- 将本地 docker 镜像转为 sif

```bash
apptainer pull docker-daemon:vasp:5.4.4
```

- 以沙盒模式构建镜像

```bash
apptainer build -s hello-world hello-world.sif
```

- 运行容器

```bash
apptainer run hello-world
```

- 在容器中运行 shell（写模式）

```bash
apptainer shell -w hello-world
```

- 执行容器中的命令（其中，容器为 vasp_5.4.4，命令为 vasp_gam）

```bash
apptainer exec vasp_5.4.4 /opt/vasp/bin/vasp_gam
```

- 映射宿主机目录到容器并执行

```bash
apptainer exec --bind $SRC/pot:$DEST/pot gvasp gvasp submit opt
```