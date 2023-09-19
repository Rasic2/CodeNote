# Go 语言教程

## MAC 系统使用 VSCode 配置 Go 开发环境

### 系统软件环境

系统配置：Apple M1 Pro，13.0 (22A380)
VSCode 版本： 1.76.1 (Universal)
go 版本：go1.20.6 darwin/arm64

### 配置步骤

1. 在 VSCode 中设置 go 环境变量 `go.goroot`

```bash
GOPROXY="https://proxy.golang.com.cn,direct"
GOROOT="/opt/homebrew/Cellar/go/1.20.6/libexec"
GOPATH="/Users/hui_zhou/go"
```

2. 启用 `Use Language Server`

:::{noot}
go.goroot 环境变量配置不对时会报 `go tool no such tool compile` 错误
:::

3. 使用 ⌘+⇧+P 快捷键调出命令面板，点击 `Go: Install/Update Tools` 来安装/更新工具

4. 使用 `go mod` 命令来 `fix inconsistent vendoring` 错误

```bash
go mod tidy
go mod vendor
```
