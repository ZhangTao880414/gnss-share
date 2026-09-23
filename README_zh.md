# GNSS 共享系统

[English](README.md) | 中文

一个 Android 客户端-服务端应用系统，通过 Wi-Fi 热点连接，将智能手机采集的 GNSS 定位数据共享给车载多媒体系统。

## 系统概述

**服务端（智能手机）：**
- 采集高精度 GNSS 定位数据
- 以后台服务方式运行，并显示前台通知
- 通过 TCP 向已连接的客户端推送定位更新
- 在通知栏中显示调试信息
- 自动管理功耗
- 支持多个客户端连接，并对每个连接进行独立的心跳监测

**客户端（车载多媒体系统）：**
- 具备自动重连的稳健连接管理
- 基于 Wi-Fi 状态感知重连
- 接收定位数据，并以系统级模拟 GPS 方式提供位置
- 在主界面显示详细的调试信息
- 显示连接状态与重连尝试情况

**语言切换：**
- 两个应用均支持英语和中文
- 点击主界面右上角的语言按钮即可切换（按钮显示目标语言名称：英文界面显示“中文”，中文界面显示"English"）
- 选择会被保存并在下次启动时自动恢复；默认跟随系统语言

## 安装说明

### 开发环境

- **JDK：** 21（在 `gradle/gradle-daemon-jvm.properties` 中声明为 Gradle 守护进程工具链；首次构建时自动下载，因此本地没有 JDK 21 也可以构建）
- **Gradle：** 9.3.1，随仓库自带的 wrapper 使用，无需本地安装 Gradle
- **Android Gradle Plugin：** 9.1.0
- **Android SDK：** API 36（compileSdk / targetSdk）；通过 Android Studio 或命令行工具安装，并设置 `ANDROID_HOME` 环境变量，或在仓库根目录创建 `local.properties` 文件并写入 `sdk.dir=<SDK 路径>`
- **minSdk：** 24（服务端）/ 28（客户端）

### 构建应用

1. **克隆并准备：**
```bash
git clone 
cd gnss-share
```

2. **构建服务端与客户端应用（debug）：**
```bash
./gradlew assembleDebug
```

3. **构建签名的 release APK**（需要仓库根目录下存在 `keystore.jks`，并设置 `KEY_PASSWORD` 环境变量）：
```bash
./gradlew assembleRelease
```

构建产物 APK 输出位置：

- `client-app/build/outputs/apk/<variant>/gnss-client-<version>.apk`
- `server-app/build/outputs/apk/<variant>/gnss-server-<version>.apk`

应用版本号取自 `VERSION_NAME` 环境变量（例如 `v2.10.2`）；未设置时使用根 `build.gradle` 中的默认版本。推送 `v*` 标签时，GitHub Actions 也会自动构建签名 release APK。

## 使用说明

### 启动系统

1. **智能手机（服务端）：**
   - 打开 "GNSS Server" 应用
   - 授予所有请求的权限
   - 点击 "Start Server"
   - 确认通知栏显示 "Server Running"
   - 服务将持续在后台运行

2. **车载系统（客户端）：**
   - 确保车机已连接手机 Wi-Fi 热点
   - 打开 "GNSS Client" 应用
   - 授予所有请求的权限
   - 应用会自动连接服务端（注意连接成功提示）
   - 确认模拟位置提供程序已生效

## 参与贡献

1. Fork 本仓库
2. 创建功能分支
3. 完成修改并提交
4. 发起 Pull Request

## 许可证

本项目基于 [GNU General Public License v3.0](LICENSE) 授权。
