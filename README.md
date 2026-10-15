<div align="center">

# 影记 ClipScribe：本地短视频转写与故事卡流水线 · douyin-story-pipeline

**输入公开视频链接，在 Windows 本机自动完成限速下载、whisper.cpp 离线转写与结构化故事卡生成。**

</div>

面向 Windows 的多平台公开视频素材研究工具。提供图形启动器（YingJiClipScribe.exe）与 PowerShell 脚本双入口，串联 yt-dlp 元数据及视频获取、FFmpeg 音频提取、本地语音转写与 Markdown 故事分析卡生成。

## 值得关注的技术

- 整合 yt-dlp + FFmpeg + whisper.cpp + Silero VAD，转写与卡片生成全流程在 Windows 本地完成
- 内置每轮批量硬限制（最高 20 条）、15–35 秒随机请求间隔与 archive 自动去重机制
- 自动生成区分来源、事实、冲突、推论与改编方案的 Markdown 故事卡，并提供 GUI 与 CMD 双入口

## 当前进度

平台可用性取决于 yt-dlp 当前提取器和访问规则，未逐个平台重新验收；转写使用本地模型，下载仍需联网。

## 使用与开发

[打开使用与开发指南](USAGE.md)，查看安装、命令、依赖和已知限制。功能状态沿用现有证据，本轮仅整理文档。
