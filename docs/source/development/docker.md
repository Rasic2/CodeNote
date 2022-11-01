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
docker rmi IMAGE1 IMAGEs ...
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

加上 `-a` 参数列出所有容器（包括未运行的），`-s` 参数列出占用空间

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
docker rm CONTAINER1 CONTAINER2 ...
```

- 查看 docker 占用空间大小

```bash
docker system df
```

加上 `-v` 参数展示详细信息

- 查看 docker 占用空间大小

```bash
docker system df
```

加上 `-v` 参数展示详细信息

- 清除 docker 构建缓存（超过 48 小时以上）

```bash
docker builder prune --filter 'until=48h'
```

## Linux 基础镜像组件（rootfs）

1. `bin` 目录

```bash
bin
├── dash
└── sh -> dash
```

2. `etc` 目录

```bash
etc
├── bash.bashrc  # 初始化 bash-shell，定义 PS1 环境变量
├── group  # 用户组配置文件，`groups` 命令必须
└── passwd  # 用户信息文件，`whoami` 命令必须
```

3. `lib` 目录

```bash
lib
├── terminfo  # 定制终端的外观和交互行为，缺失时 backspace 删除变空格
└── x86_64-linux-gnu
    ├── ld-2.31.so  # 程序运行前链接动态库
    ├── libc-2.31.so
    ├── libc.so.6 -> libc-2.31.so
    ├── libdl-2.31.so  # 程序运行时链接动态库
    ├── libdl.so.2 -> libdl-2.31.so
    ├── libgcc_s.so.1
    ├── liblzma.so.5 -> liblzma.so.5.2.4
    ├── liblzma.so.5.2.4
    ├── libm-2.31.so
    ├── libm.so.6 -> libm-2.31.so
    ├── libncurses.so.6 -> libncurses.so.6.2
    ├── libncurses.so.6.2
    ├── libncursesw.so.6 -> libncursesw.so.6.2
    ├── libncursesw.so.6.2
    ├── libnss_files-2.31.so  # whoami 命令必须
    ├── libnss_files.so.2 -> libnss_files-2.31.so
    ├── libpcre.so.3 -> libpcre.so.3.13.3
    ├── libpcre.so.3.13.3
    ├── libpcre2-8.so.0 -> libpcre2-8.so.0.9.0
    ├── libpcre2-8.so.0.9.0
    ├── libpthread-2.31.so
    ├── libpthread.so.0 -> libpthread-2.31.so
    ├── librt-2.31.so
    ├── librt.so.1 -> librt-2.31.so
    ├── libselinux.so.1
    ├── libstdc++.so.6 -> libstdc++.so.6.0.28
    ├── libstdc++.so.6.0.28
    ├── libtinfo.so.6 -> libtinfo.so.6.2
    ├── libtinfo.so.6.2
    ├── libunwind-ptrace.so.0 -> libunwind-ptrace.so.0.0.0
    ├── libunwind-ptrace.so.0.0.0  # strace 命令依赖
    ├── libunwind-x86_64.so.8 -> libunwind-x86_64.so.8.0.1
    ├── libunwind-x86_64.so.8.0.1 # strace 命令依赖
    ├── libunwind.so.8 -> libunwind.so.8.0.1
    └── libunwind.so.8.0.1 # strace 命令依赖
```

4. `lib64` 目录

```bash
lib64/
└── ld-linux-x86-64.so.2 -> /lib/x86_64-linux-gnu/ld-2.31.so
```

5. `usr` 目录

```bash
usr
└── bin
    ├── bash
    ├── grep  # mpirun 必须
    ├── groups
    ├── ls
    ├── strace
    ├── stty
    ├── uname  # mpirun 必须
    └── whoami  # mpirun 必须
```

## Intel-oneapi 镜像组件

1. `mkl` 和 `mpi` 的库文件和可执行程序

```bash
oneapi
├── mkl
│   └── 2022.2.0
│       └── lib
│           └── intel64
│               ├── [313K] libmkl_blacs_intelmpi_lp64.so.2
│               ├── [ 71M] libmkl_core.so.2
│               ├── [ 40M] libmkl_def.so.2
│               ├── [ 20M] libmkl_intel_lp64.so.2
│               └── [ 28M] libmkl_sequential.so.2
└── mpi
    └── 2021.7.0
        ├── bin
        │   ├── IMB-MPI1
        │   ├── IMB-MPI1-GPU
        │   ├── IMB-MT
        │   ├── IMB-NBC
        │   ├── IMB-P2P
        │   ├── IMB-RMA
        │   ├── cpuinfo
        │   ├── hydra_bstrap_proxy
        │   ├── hydra_nameserver
        │   ├── hydra_pmi_proxy
        │   ├── impi_info
        │   ├── mpicc
        │   ├── mpicxx
        │   ├── mpiexec -> mpiexec.hydra
        │   ├── mpiexec.hydra
        │   ├── mpif77
        │   ├── mpif90
        │   ├── mpifc
        │   ├── mpigcc
        │   ├── mpigxx
        │   ├── mpiicc
        │   ├── mpiicpc
        │   ├── mpiifort
        │   ├── mpirun
        │   ├── mpitune
        │   └── mpitune_fast
        ├── lib
        │   ├── libmpi_shm_heap_proxy.so
        │   ├── libmpicxx.a
        │   ├── libmpicxx.so -> libmpicxx.so.12.0.0
        │   ├── libmpicxx.so.12 -> libmpicxx.so.12.0.0
        │   ├── libmpicxx.so.12.0 -> libmpicxx.so.12.0.0
        │   ├── libmpicxx.so.12.0.0
        │   ├── libmpifort.so -> libmpifort.so.12.0.0
        │   ├── libmpifort.so.12 -> libmpifort.so.12.0.0
        │   ├── libmpifort.so.12.0 -> libmpifort.so.12.0.0
        │   ├── libmpifort.so.12.0.0
        │   ├── libmpijava.so -> libmpijava.so.1
        │   ├── libmpijava.so.1 -> libmpijava.so.1.0
        │   ├── libmpijava.so.1.0
        │   ├── libtvmpi.so -> libtvmpi.so.12
        │   ├── libtvmpi.so.12 -> libtvmpi.so.12.0.0
        │   ├── libtvmpi.so.12.0.0
        │   ├── mpi.jar
        │   └── release
        │       ├── [  16]  libmpi.so -> libmpi.so.12.0.0
        │       ├── [  16]  libmpi.so.12 -> libmpi.so.12.0.0
        │       ├── [  16]  libmpi.so.12.0 -> libmpi.so.12.0.0
        │       ├── [ 15M]  libmpi.so.12.0.0
        │       ├── [362K]  libmpi_ilp64.a
        │       ├── [  19]  libmpi_ilp64.so -> libmpi_ilp64.so.4.1
        │       ├── [  19]  libmpi_ilp64.so.4 -> libmpi_ilp64.so.4.1
        │       └── [357K]  libmpi_ilp64.so.4.1
        └── libfabric
            ├── bin
            │   ├── fi_info
            │   └── fi_pingpong
            └── lib
                ├── libfabric.so -> libfabric.so.1
                ├── libfabric.so.1
                └── prov
                    ├── libefa-fi.so
                    ├── libmlx-fi.so
                    ├── libpsm3-fi.so
                    ├── libpsmx2-fi.so
                    ├── librxm-fi.so
                    ├── libshm-fi.so
                    ├── libsockets-fi.so
                    ├── libtcp-fi.so
                    ├── libverbs-1.1-fi.so
                    └── libverbs-1.12-fi.so
```

2. 环境变量设置

```bash
ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/bin:${PATH}"
ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/bin:${PATH}"

ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mkl/2022.2.0/lib/intel64:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib/release:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib:${LD_LIBRARY_PATH}"

# Very Important!!!
ENV FI_PROVIDER_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib/prov:${FI_PROVIDER_PATH}"
```

## 构建 VASP 镜像（5.4.4 版本）

1. 首先拉取 `intel/oneapi-hpckit` 基础镜像

```
docker pull intel/oneapi-hpckit
```

2. 在这个基础镜像中将 `VASP` 编译出来

3. 编写 `Dockerfile` 从空白镜像构建 `VASP` 镜像（最终大小为 260M+），如下：

```bash
From scratch
LABEL maintainer="Hui Zhou"

COPY build/bin /bin
COPY build/usr /usr
COPY build/etc /etc
COPY build/lib /lib
COPY build/lib64 /lib64

COPY build/oneapi /opt/intel/oneapi
COPY build/vasp_gam /opt/vasp/bin/vasp_gam

ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/bin:${PATH}"
ENV PATH="/opt/intel/oneapi/mpi/2021.7.0/bin:${PATH}"

ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mkl/2022.2.0/lib/intel64:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0//lib/release:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib:${LD_LIBRARY_PATH}"

ENV FI_PROVIDER_PATH="/opt/intel/oneapi/mpi/2021.7.0/libfabric/lib/prov:${FI_PROVIDER_PATH}"

WORKDIR /opt/vasp/bin/

CMD ["/usr/bin/bash"]
```

## 构建 GVasp 镜像（0.1.2 版本）

1. 首先拉取 `centos7` 基础镜像

2. 在该镜像中使用 `conda` 安装 `GVasp`

3. 编写 `Dockerfile` 从空白镜像构建 `GVasp` 镜像（最终大小为 320M+），如下（`build` 文件夹结构可以看<a href="../_static/gvasp_build" target="_blank">这里</a>）：

```bash
From scratch
LABEL maintainer="Hui Zhou"

COPY build/bin /bin
COPY build/usr /usr
COPY build/lib64 /lib64
COPY build/home /home

ENV PATH="/home/hzhou/anaconda3/envs/gvasp/bin:${PATH}"

CMD ["/usr/bin/bash"]
```
