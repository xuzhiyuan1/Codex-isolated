# Codex2 隔离版

这是一个用于在同一台 Mac 上运行独立 Codex 登录环境的私有安装包仓库。

应用打开后显示为 **Codex**，但它使用独立的浏览器登录目录和 Codex 数据目录，不会与原版 Codex 共用账号状态。

## 当前版本

- 最新版本：`Codex2-v0.4.zip`
- 仓库只保留根目录的最新安装包；历史版本不随仓库保存
- 支持设备：Apple 芯片 Mac（M1 或更新）
- 最低系统版本：macOS 12.0

## 下载和安装

该仓库使用 Git LFS 保存大型 ZIP。首次下载前请安装 Git LFS：

```bash
brew install git-lfs
git lfs install
git clone git@github.com:xuzhiyuan1/Codex-isolated.git
cd Codex-isolated
```

解压最新版：

```bash
ditto -x -k Codex2-v0.4.zip .
```

然后把解压得到的 `Codex.app` 拖入“应用程序”文件夹。首次启动时，可以在 Finder 中右键应用并选择“打开”。

## 账号隔离说明

隔离版只改变启动环境：

- 浏览器登录状态保存在 `~/Library/Application Support/Codex2-Private/browser-data`
- Codex 数据保存在 `~/Library/Application Support/Codex2-Private/codex-home`

安装包不包含当前电脑的账号、Cookie、数据库或聊天记录。在另一台 Mac 首次启动时，需要重新登录要隔离使用的账号。

## 版本说明

### v0.4

- 更新至官方 Codex `26.901.51231 / 8109`
- 保留蓝紫色 Codex 应用图标
- 保留独立账号环境
- 不包含水印、皮肤或输入框样式修改

### v0.3

- 更新至官方 Codex `26.818.41509 / 6962`
- 保留蓝紫色 Codex 应用图标和独立账号环境
- 不包含水印、皮肤或输入框样式修改

### v0.2

- 更新至官方 Codex `26.814.41407 / 6720`
- 保留 v0.1 的蓝紫色 Codex 应用图标
- 保留独立账号环境
- 不包含水印、皮肤或输入框样式修改

### v0.1

- 保留 v0.0 的原版界面和独立账号环境
- 应用名称仍为 `Codex`
- 更换为蓝紫色 Codex 应用图标
- 不包含水印、皮肤或输入框样式修改

### v0.0

- 从原版 ChatGPT/Codex 应用重新部署
- 应用名称为 `Codex`
- 保留独立账号环境
- 界面资源与原版一致
- 不包含水印、皮肤或输入框样式修改

## 文件校验

```text
60f24b50e6823cbe691082168aee6e472b1e077a59e82bd12566ca017e4112fe  Codex2-v0.4.zip
```

可以使用下面的命令核对：

```bash
shasum -a 256 Codex2-v0.4.zip
```
