# Docker 教程

[Docker](https://www.docker.com/) 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。它可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

## Docker 常用命令

- 从镜像仓库中拉取镜像（IMAGE）

```bash
docker pull IMAGE
```

- 查看所有镜像

```bash
docker images
```

- 删除镜像

```bash
docker image rm IMAGE1 IMAGE2 ...
```

- 根据 `Dockerfile` 构建镜像（如 vasp:5.4.4）

```bash
docker build -t vasp:5.4.4 .
```

最后的 `.` 表示 Dockerfile 在当前目录下面。

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

- 容器重命名

```bash
docker rename old_name new_name
```

- 退出 docker 终端（快捷键`Ctrl+P+Q`）

- 终止一个容器

```bash
docker stop container_id
```

- 删除容器

```bash
docker container rm CONTAINER1 CONTAINER2 ...
```

## 构建 VASP 镜像（5.4.4 版本）

1. 首先拉取 intel/oneapi-hpckit 基础镜像

```
docker pull intel/oneapi-hpckit
```

2. 在这个基础镜像中将 VASP 编译出来

3. 编写 Dockerfile 从空白镜像构建 VASP 镜像，如下：

```bash
From scratch
LABEL maintainer="Hui Zhou"
COPY build/vasp_gam /opt/vasp/bin/vasp_gam
COPY build/libmkl_intel_lp64.so.2 /opt/intel/oneapi/mkl/2022.2.0/lib/intel64/libmkl_intel_lp64.so.2
COPY build/libmkl_sequential.so.2 /opt/intel/oneapi/mkl/2022.2.0/lib/intel64/libmkl_sequential.so.2
COPY build/libmkl_core.so.2 /opt/intel/oneapi/mkl/2022.2.0/lib/intel64/libmkl_core.so.2
COPY build/libmkl_blacs_intelmpi_lp64.so.2 /opt/intel/oneapi/mkl/2022.2.0/lib/intel64/libmkl_blacs_intelmpi_lp64.so.2
COPY build/fi_info /opt/intel/oneapi/mpi/2021.7.0/libfabric/bin/fi_info
COPY build/fi_pingpong /opt/intel/oneapi/mpi/2021.7.0/libfabric/bin/fi_pingpong
COPY build/mpi_bin /opt/intel/oneapi/mpi/2021.7.0/bin
COPY build/mpi_lib /opt/intel/oneapi/mpi/2021.7.0/lib
COPY build/libfabric_lib /opt/intel/oneapi/mpi/2021.7.0/libfabric/lib

COPY build/libc-2.31.so /lib/x86_64-linux-gnu/libc-2.31.so
COPY build/libc.so.6 /lib/x86_64-linux-gnu/libc.so.6
COPY build/libstdc++.so.6 /lib/x86_64-linux-gnu/libstdc++.so.6
COPY build/libstdc++.so.6.0.28 /lib/x86_64-linux-gnu/libstdc++.so.6.0.28

COPY build/librt.so.1 /lib/x86_64-linux-gnu/librt.so.1
COPY build/librt-2.31.so /lib/x86_64-linux-gnu/librt-2.31.so
COPY build/libm.so.6 /lib/x86_64-linux-gnu/libm.so.6
COPY build/libm-2.31.so /lib/x86_64-linux-gnu/libm-2.31.so
COPY build/ld-2.31.so /lib/x86_64-linux-gnu/ld-2.31.so
COPY build/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2
COPY build/libgcc_s.so.1 /lib/x86_64-linux-gnu/libgcc_s.so.1

COPY build/libselinux.so.1 /lib/x86_64-linux-gnu/libselinux.so.1
COPY build/libpcre2-8.so.0.9.0 /lib/x86_64-linux-gnu/libpcre2-8.so.0.9.0
COPY build/libpcre2-8.so.0 /lib/x86_64-linux-gnu/libpcre2-8.so.0
COPY build/libdl.so.2 /lib/x86_64-linux-gnu/libdl.so.2
COPY build/libdl-2.31.so /lib/x86_64-linux-gnu/libdl-2.31.so
COPY build/libpthread.so.0 /lib/x86_64-linux-gnu/libpthread.so.0
COPY build/libpthread-2.31.so /lib/x86_64-linux-gnu/libpthread-2.31.so

COPY build/ls /usr/bin/ls
COPY build/sh /bin/sh
COPY build/dash /bin/dash

ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mkl/2022.2.0/lib/intel64:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib/release:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib:${LD_LIBRARY_PATH}"

ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/bin:${PATH}"
ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/bin:${PATH}"

ENV FI_PROVIDER_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib/prov:${FI_PROVIDER_PATH}"

WORKDIR /opt/vasp/bin/
ENTRYPOINT ["/opt/vasp/bin/vasp_gam"]
```
