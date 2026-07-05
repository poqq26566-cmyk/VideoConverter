# 视频转MP4 (硬件加速)

用 Google 官方 `androidx.media3-transformer` 库做转码，走系统硬件编码器（MediaCodec），
不依赖已停止维护的 FFmpegKit。

## 使用方法

### 1. 推送到 GitHub
```bash
cd VideoConverter
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin <你的GitHub仓库地址>
git push -u origin main
```

### 2. 自动编译
推送后 GitHub Actions 会自动触发（`.github/workflows/build.yml`）。
去仓库的 **Actions** 标签页，点进最新的 workflow run，等它跑完（通常几分钟）。

### 3. 下载 APK
在该 workflow run 页面下方的 **Artifacts** 里，下载 `app-debug-apk`，
里面是 `app-debug.apk`。

### 4. 安装到手机
把 APK 传到手机上，打开时系统会提示"允许安装未知来源应用"，允许后安装即可。

## 功能
- 点击按钮选择本地视频文件（avi/mov/wmv 等）
- 自动用硬件加速转码为 MP4
- 转换完成后自动保存到手机的 Downloads 文件夹

## 已知限制 / 可能需要微调
- `media3-transformer` 版本号锁定在 `1.4.1`，如果编译时提示 API 不兼容
  （比如 `Transformer.Listener` 的方法签名变化），去 Google 官方文档
  搜索对应版本的 `Transformer` 用法，调整 `MainActivity.kt` 里
  `onCompleted` / `onError` 的参数即可，核心逻辑不需要大改。
- 首次跑 GitHub Actions 如果因为 Android SDK License 报错，
  可以在 workflow 里加一步用 `android-actions/setup-android` 显式安装 SDK。
- WMV 一些老旧编码硬件解码器不一定支持，遇到解码失败可尝试先用其他工具
  预处理，或退回软件解码路径（改用系统自带的 `MediaExtractor` + 软解）。

## 为什么不用 FFmpegKit
FFmpegKit 已于 2025 年 1 月宣布退役，4 月起所有预编译包从 Maven Central/CocoaPods/npm 下架，
现在直接用它的项目大概率会在 Gradle 依赖解析阶段直接报错。所以这里换成了 Google 仍在维护的
`media3-transformer`。
