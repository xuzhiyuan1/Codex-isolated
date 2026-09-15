# Codex2 隔离版维护说明

## 仓库目标

本仓库维护 macOS Codex 的独立账号副本。应用界面应与上游原版保持一致；隔离只通过启动参数和环境变量实现。

## 当前发布结构

```text
README.md
AGENTS.md
Codex2-v0.5.zip
```

仓库只保留根目录的最新 ZIP。发布新版本时，直接用新的 `Codex2-vX.Y.zip` 替换旧包；历史版本只保留在 Git 提交历史中，不另存 ZIP。

所有 ZIP 必须由 Git LFS 管理，不得作为普通 Git Blob 提交。

## 隔离机制

启动器位于应用包的 `Contents/MacOS/Codex`，只负责：

1. 设置 `CODEX_HOME=~/Library/Application Support/Codex2-Private/codex-home`。
2. 传入 `--user-data-dir=~/Library/Application Support/Codex2-Private/browser-data`。
3. 传入 `--class=Codex`。
4. 执行原版 `Contents/MacOS/ChatGPT`。

应用元数据：

- `CFBundleDisplayName=Codex`
- `CFBundleName=Codex`
- `CFBundleExecutable=Codex`
- `CFBundleIdentifier=local.codex.private`

不要把隔离目录改回原版 Codex 的目录，否则两个账号会重新共享登录状态。

## 不得打包的数据

严禁把下列目录或其中任何文件提交、压缩或上传：

```text
~/Library/Application Support/Codex2-Private/
~/Library/Application Support/Codex/
```

这些目录可能包含账号状态、Cookie、本地数据库、聊天记录和其他隐私数据。发布包只能包含 `.app`。

## 重建原则

1. 从当前官方 `/Applications/ChatGPT.app` 复制一个全新应用包。
2. 不复用旧版本的 `app.asar`、CSS、图片或其他界面资源。
3. 只修改 `Info.plist`、增加隔离启动器，以及按版本要求替换应用图标；不得修改 `app.asar` 或聊天界面资源。
4. 启动器必须使用当前用户的主目录动态计算隔离路径，不能硬编码用户名。
5. 修改应用包后，使用本机临时签名：

   ```bash
   codesign --force --deep --sign - Codex.app
   ```

6. 使用 macOS 保真 ZIP 打包：

   ```bash
   ditto -c -k --sequesterRsrc --keepParent Codex.app Codex2-vX.Y.zip
   ```

不要使用 `zip -0`，它不压缩内容，会把安装包扩大到约 2GB。

## 发布前验证

每次发布至少完成以下检查：

```bash
cmp -s /Applications/ChatGPT.app/Contents/Resources/app.asar Codex.app/Contents/Resources/app.asar
codesign --verify --deep --strict --verbose=2 Codex.app
unzip -t Codex2-vX.Y.zip
git lfs ls-files
```

还应实际启动应用并确认：

- 应用名称显示为 `Codex`。
- 输入框和聊天界面与原版一致。
- 独立数据库位于 `Codex2-Private/codex-home`。
- 原版 Codex 与隔离版可以分别登录不同账号。

新版本验证成功前，不得删除现有可用版本。验证成功后，删除仓库工作区中的旧 ZIP，只保留最新版本。

## 当前版本状态

- `v0.5`：更新至官方 Codex `26.908.70816 / 9275`，沿用蓝紫色图标和隔离机制。
- `v0.4`：更新至官方 Codex `26.901.51231 / 8109`，沿用蓝紫色图标和隔离机制。
- `v0.3`：更新至官方 Codex `26.818.41509 / 6962`。
- `v0.2`：更新至官方 Codex `26.814.41407 / 6720`。
- `v0.1`：干净界面和隔离机制，仅更换蓝紫色 Codex 应用图标。
- `v0.0`：首个干净重建版。
- 所有版本均无水印、无皮肤、无输入框或聊天界面补丁。
- 当前构建目标：Apple Silicon（arm64），macOS 12.0 或更新版本。
