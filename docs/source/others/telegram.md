# telegram 教程

[Telegram](https://telegram.org/) 是跨平台的即时通讯软件，其客户端是自由及开放源代码软件，但服务器端是专有软件。用户可以相互交换加密与自毁消息（类似于“阅后即焚”），发送照片、影片等所有类型文件。

## telegram 不限速下载上传工具

使用开源软件 [tdl](https://github.com/iyear/tdl) 可以实现 telegram 的不限速下载和上传，常用法如下：

### 环境设置

- 指定账户

```bash
tdl -n account 或者 export TDL_NS=account
```

- 指定代理

```bash
tdl --proxy socks5://localhost:1080 或者 export TDL_PROXY=socks5://localhost:1080
```

### 登录账户

```bash
tdl login -d /path/to/Telegram_Desktop
```

### 聊天

- 查看聊天对象

```bash
tdl chat ls
```

- 导出群组所有内容到 json 文件（默认为 tdl-export.json）

```bash
tdl chat export -c CHAT_ID -o example.json
```

### 下载

- 下载 json 文件中的所有内容

```bash
tdl dl -f *.json
```

- 下载链接内容

```bash
tdl dl -u https://t.me/*
```

- 跳过重复文件

```bash
tdl dl -u https://t.me/* --skip-same
```

- 下载指定后缀文件

```bash
tdl dl -u https://t.me/* --skip-same -i jpg,png
```

- 排除指定后缀文件

```bash
tdl dl -u https://t.me/* --skip-same -e mp4,flv
```
