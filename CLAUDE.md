# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

VoiceInk 是一个原生的 macOS 语音转文字应用程序，使用本地 AI 模型实现实时语音转录。该项目采用 SwiftUI + Swift 5 技术栈，支持多种转录模型和云服务。

## 构建和开发

### 构建要求
- macOS 14.0 或更高版本
- Xcode（最新版本推荐）
- Swift（最新版本推荐）

### 构建步骤

1. **构建 whisper.cpp Framework**
   ```bash
   git clone https://github.com/ggerganov/whisper.cpp.git
   cd whisper.cpp
   ./build-xcframework.sh
   ```
   这将在 `build-apple/whisper.xcframework` 创建 XCFramework

2. **添加 whisper.xcframework 到项目**
   - 将 `../whisper.cpp/build-apple/whisper.xcframework` 拖拽到项目导航器
   - 或在项目设置的"Frameworks, Libraries, and Embedded Content"部分手动添加

3. **构建和运行**
   - 使用 Cmd+B 或 Product > Build 构建项目
   - 使用 Cmd+R 或 Product > Run 运行项目

### 常用开发命令
- **清理构建**: Cmd+Shift+K
- **清理构建缓存**: Cmd+Shift+K 两次
- **运行测试**: 在 Xcode 中使用 Product > Test 或 Cmd+U

## 架构和代码结构

### 核心应用结构
- **VoiceInkApp.swift**: 主应用入口点，配置所有全局服务和状态管理
- **AppDelegate.swift**: 处理应用生命周期和菜单栏模式切换

### 主要模块

#### Services (服务层)
- **TranscriptionService**: 转录服务的抽象基类
- **LocalTranscriptionService**: 本地转录服务（Whisper 模型）
- **CloudTranscriptionService**: 云转录服务（支持多个提供商）
- **AIService**: AI 增强服务
- **AudioDeviceManager**: 音频设备管理
- **ScreenCaptureService**: 屏幕截图服务
- **HotkeyManager**: 全局快捷键管理

#### Models (数据模型)
- **Transcription**: 转录记录的 SwiftData 模型
- **TranscriptionModel**: 转录模型配置
- **PowerModeConfig**: Power Mode 配置

#### Views (视图层)
- **ModelManagementView**: AI 模型管理界面
- **PowerModeView**: Power Mode 配置界面
- **AudioTranscribeView**: 音频转录界面

#### Power Mode 系统
- **ActiveWindowService**: 活动窗口检测
- **BrowserURLService**: 浏览器 URL 获取
- **PowerModeSessionManager**: Power Mode 会话管理

### 依赖管理
项目使用 Swift Package Manager 管理以下主要依赖：
- **Sparkle**: 自动更新
- **KeyboardShortcuts**: 自定义快捷键
- **LaunchAtLogin**: 登录启动
- **MediaRemoteAdapter**: 媒体播放控制
- **Zip**: 文件压缩工具

### 数据持久化
- 使用 **SwiftData** 进行本地数据存储
- 数据存储位置: `~/Library/Application Support/com.prakashjoshipax.VoiceInk/`

### 转录引擎
- **whisper.cpp**: OpenAI Whisper 模型的高性能推理
- **FluidAudio**: Parakeet 模型实现
- 支持多种云转录服务：OpenAI、Deepgram、ElevenLabs、Gemini、Groq、Mistral

### 开发注意事项
- 应用支持菜单栏模式和传统窗口模式
- 实现了完整的隐私保护（100% 离线处理选项）
- 集成了屏幕上下文感知功能
- 支持个人词典和智能文本替换

### 测试
- **VoiceInkTests**: 单元测试目标
- **VoiceInkUITests**: UI 测试目标
- 在开发前运行测试套件，确保修改后所有测试通过