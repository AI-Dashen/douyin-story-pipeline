# 影记 ClipScribe 使用与开发指南

[返回项目首页](README.md)

## Windows 一键使用

普通用户不需要安装 Git，也不需要输入命令：

1. 点击 GitHub 页面右上角的 `Code`。
2. 选择 `Download ZIP`。
3. 解压下载的 ZIP。
4. 如果下载的是 Release 版本，优先双击 `YingJiClipScribe.exe`。
5. 如果没有 exe，或者 exe 被安全软件拦截，可以双击备用入口 `START.cmd`。

图形界面里按顺序操作：

1. 首次点击 `安装或修复工具`。
2. 点击 `编辑视频链接`，每行粘贴一个公开视频链接并保存。
3. 点击 `开始处理素材`。

命令行菜单入口也可以使用：

1. 双击 `START.cmd`。
2. 首次选择 `1. 首次安装或修复工具`。
3. 安装完成后选择 `2. 编辑视频链接`，每行粘贴一个公开视频链接并保存。
4. 选择 `3. 开始下载、转写并生成故事卡`。

首次安装大约下载 400MB 的工具和模型。安装完成后，程序可以独立运行；下载新视频时仍然需要网络，转写过程在本机完成。

结果可以直接从图形界面或菜单打开：

- `4. 打开下载的视频`
- `5. 打开故事卡`

## 工作流程

```text
人工选择多平台公开视频
-> 保守下载视频与来源元数据
-> 本地提取音频
-> whisper.cpp 中文转写
-> 生成 Markdown 故事卡
-> 事实核验、分析和原创写作
```

## 环境

- Windows 10 或 Windows 11
- Windows PowerShell 5.1 或 PowerShell 7
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [FFmpeg](https://ffmpeg.org/download.html)
- [whisper.cpp](https://github.com/ggml-org/whisper.cpp)
- whisper.cpp 模型，例如 `ggml-small-q5_1.bin`
- Silero VAD 模型，例如 `ggml-silero-v6.2.0.bin`

工具可以安装到系统 `PATH`。也可以采用下面的本地结构：

```text
.tools/
  yt-dlp.exe
  ffmpeg.exe
  whisper-cli.exe
  models/
    ggml-small-q5_1.bin
    ggml-silero-v6.2.0.bin
```

`.tools` 会被 Git 忽略。

## 打包 exe

项目的 exe 是一个 Windows 图形启动器，核心处理逻辑仍然放在 PowerShell 脚本里。这样做的好处是：

- 普通用户双击 exe 使用。
- 开发者仍然可以直接阅读和修改脚本。
- 下载的视频、模型和用户链接会保留在项目文件夹中，不会因为 exe 每次自解压而丢失。

在 Windows 上可以运行：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\build-windows-exe.ps1
```

生成：

```text
YingJiClipScribe.exe
```

## 快速开始

1. 检查工具：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\check-tools.ps1
```

2. 第一次使用时创建链接清单：

```powershell
Copy-Item .\urls.example.txt .\urls.txt
```

3. 将你有权研究的公开视频链接写入 `urls.txt`，一行一个。

4. 运行完整流程：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\run.ps1
```

结果位于：

```text
data/raw/           原始视频、封面和来源元数据
data/transcripts/   本地语音转写
data/cards/         Markdown 故事分析卡
data/archive.txt    下载去重记录
```

## 分步运行

只下载：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\download.ps1
```

指定本轮最多下载 3 条：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass `
  -File .\download.ps1 `
  -BatchSize 3
```

只转写：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\transcribe.ps1
```

重新生成全部故事卡：

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass `
  -File .\build-queue.ps1 `
  -Force
```

## 使用边界

- 仅处理公开可访问或你已取得授权的内容。
- 遵守所在地法律、平台条款和原作者权利。
- 不绕过验证码、登录限制、付费墙或其他访问控制。
- 出现验证、限流或异常响应时停止，不进行高频重试。
- 下载不等于获得再发布权。最终作品应进行事实核验和真正的原创表达。
- 不直接转载原视频、原音频、完整转写或大段原文。
- 默认匿名化普通人物的面部、姓名、联系方式和精确地址。

## 隐私与 Git

`.gitignore` 默认排除：

- 下载的视频和封面
- 平台原始 JSON 与临时鉴权字段
- Whisper 模型和二进制工具
- 转写、分析卡和个人链接清单

提交前仍应执行一次敏感信息检查。不要把 Cookie、账号令牌、私钥或个人资料加入仓库。

## 许可证

项目脚本使用 [MIT License](LICENSE)。视频、模型、转写和第三方工具不包含在该许可证中，各自遵循其原始许可与权利归属。
