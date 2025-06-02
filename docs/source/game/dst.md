# 饥荒联机版

## 饥荒联机版服务器搭建（Linux）

### 1. Bohrium 上创建一个容器节点

机型：2 核 4GB
镜像：ubuntu:22.04-py3.10

### 2. 配置更新源

```bash
add-apt-repository multiverse
dpkg --add-architecture i386
apt-get -y update
```

### 3. 安装依赖

```bash
apt-get -y install lib32gcc-s1 libstdc++6:i386 libgcc1:i386 libcurl4-gnutls-dev:i386
```

### 4. 安装 SteamCMD

```bash
# 创建文件夹
cd ~
mkdir -p dst-server/steamcmd

# 安装 steamcmd 程序
cd ~/dst-server/steamcmd/
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -

# 执行安装程序，安装完毕输入 quit 退出
./steamcmd.sh
```

### 5. 安装饥荒服务端

```bash
# 创建游戏安装文件夹
cd ~
mkdir -p dst-server/dontstarve

# 安装游戏
~/dst-server/steamcmd/steamcmd.sh +force_install_dir ~/dst-server/dontstarve +login anonymous +app_update 343050 validate +quit
```

### 6. 创建服务配置

- Steam 账号登陆[科雷官网](https://link.zhihu.com/?target=https%3A//accounts.klei.com/login)

![alt text](image-6.png)

- 登录之后选择【游戏】-【《饥荒：联机版》的游戏服务器】

![alt text](image.png)

- 添加新服务器，输入【你的服务器名称】，然后点击【添加服务器】

![alt text](image-1.png)

- 添加完成后，选择【配置服务器】

![alt text](image-2.png)

- 输入基础的信息，然后点击【下载设置】

![alt text](image-3.png)

- 下载完成后，得到一个名为“MyDediServer.zip”的压缩文件。解压文件后，文件夹的结构如下：

![alt text](image-5.png)

### 7. 配置服务器

```bash
# 创建游戏配置文件夹
cd ~
mkdir -p dst-server/dontstarve-config/clusters

# 上传配置文件并解压
cd dst-server/dontstarve-config/clusters
unzip MyDediServer.zip

```

解压之后的目录结构如下所示：
![alt text](image-7.png)

【cluster.ini】：集群的配置

【cluster_token.txt】：服务器 token

【Caves】:洞穴相关的配置

【Master】：地面相关配置

### 8. 启动游戏服务器

- 在 `~/dst-server` 目录下创建游戏启动脚本 `run_server.sh`

```bash
#!/bin/bash

# 以下需要替换为你自己的路径
game_home=/home/tom/dst-server/dontstarve
game_config_dir=/home/tom/dst-server/dontstarve-config
game_log_path=/home/tom/dst-server/log

shard=$1
if [[ "$shard" == "Master" ]]; then
    echo "启动地面服务器"
elif [[ "$shard" == "Caves" ]]; then
    echo "启动洞穴服务器"
else
    echo "输入只能是Master或者Caves"
    exit -1
fi

cd "$game_home"/bin
run_command=(./dontstarve_dedicated_server_nullrenderer)
run_command+=(-console)
run_command+=(-persistent_storage_root "$game_config_dir")
run_command+=(-conf_dir clusters)
run_command+=(-cluster MyDediServer)
run_command+=(-shard "$shard")

mkdir -p "$game_log_path"
"${run_command[@]}" >"$game_log_path"/log_$shard.log 2>&1 &
```

- 启动服务器

```bash
chmod +x run_server.sh
./run_server.sh Master # 启动地面
./run_server.sh Caves # 启动洞穴
```

### 9. 停止服务器

```bash
ps -ef | grep dontstarve
kill -9 <PID>

```

### 参考资料

- [饥荒联机版专用服务器搭建教程（二）【Linux 篇】](https://zhuanlan.zhihu.com/p/1896332482762236476)
