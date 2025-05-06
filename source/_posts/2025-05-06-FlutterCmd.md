---
title: Flutter 常用命令整理
date: 2025-05-06
tag: [Flutter,cmd]
category: Flutter
---


Flutter 提供了丰富的命令行工具和包管理功能，以下是常用的命令和包管理操作整理：

---

# 一、常用 Flutter 命令
1. **项目创建与运行**
• 创建新项目  

  ```bash
  flutter create <project_name>
  ```
  • 例如：`flutter create my_app`


• 运行项目  

  ```bash
  flutter run
  ```
  • 附加参数：

    ◦ `-d <device_id>`：指定设备（通过 `flutter devices` 查看设备列表）。

    ◦ `--release`：以发布模式运行。

    ◦ `--debug`：调试模式（默认）。

    ◦ `--profile`：性能分析模式。


2. **构建应用**
• 构建 APK（Android）  

  ```bash
  flutter build apk
  ```
  • 分屏优化：`flutter build apk --split-per-abi`


• 构建 App Bundle（Android）  

  ```bash
  flutter build appbundle
  ```

• 构建 IPA（iOS）  

  ```bash
  flutter build ipa
  ```

3. **设备与模拟器**
• 列出连接的设备  

  ```bash
  flutter devices
  ```

• 启动模拟器  

  ```bash
  flutter emulators --launch <emulator_name>
  ```

4. **依赖与工具**
• 检查环境配置  

  ```bash
  flutter doctor
  ```

• 升级 Flutter SDK  

  ```bash
  flutter upgrade
  ```

• 清理缓存  

  ```bash
  flutter clean
  ```

5. **测试与调试**
• 运行单元测试  

  ```bash
  flutter test
  ```

• 运行集成测试  

  ```bash
  flutter test integration_test/
  ```

---

# 二、包管理（`pubspec.yaml`）
Flutter 使用 `pub` 工具管理依赖，配置文件为 `pubspec.yaml`。

1. **添加依赖**
在 `pubspec.yaml` 的 `dependencies` 或 `dev_dependencies` 中添加包：
```yaml
dependencies:
  http: ^1.1.0  # 版本号遵循语义化版本
  provider: ^6.0.0
```

2. **常用包管理命令**
• 安装依赖  

  ```bash
  flutter pub get
  ```

• 升级依赖  

  ```bash
  flutter pub upgrade
  ```

• 升级到最新兼容版本  

  ```bash
  flutter pub upgrade --major-versions
  ```

• 查看过期的依赖  

  ```bash
  flutter pub outdated
  ```

3. **依赖版本控制**
• `^1.2.3`：允许升级到兼容版本（如 `1.x.x`，但不包括 `2.0.0`）。

• `any`：使用最新版本（不推荐，可能导致冲突）。

• 直接指定版本：`1.2.3`（严格锁定）。


---

# 三、常用第三方包推荐
1. 网络请求  
   • `http`：轻量级 HTTP 客户端。

   • `dio`：功能强大的网络库（支持拦截器、文件上传等）。


2. 状态管理  
   • `provider`：官方推荐的轻量级状态管理。

   • `riverpod`：`provider` 的增强版。

   • `bloc`：基于事件驱动的状态管理。


3. 路由导航  
   • `go_router`：声明式路由管理。

   • `auto_route`：代码生成路由。


4. UI 组件  
   • `flutter_svg`：渲染 SVG 图片。

   • `cached_network_image`：缓存网络图片。


5. 本地存储  
   • `shared_preferences`：轻量级键值存储。

   • `hive`：高性能本地数据库。


6. 工具类  
   • `intl`：国际化支持。

   • `equatable`：简化对象比较。


---

# 四、其他实用命令
• 生成图标和启动页  

  ```bash
  flutter pub run flutter_launcher_icons:main
  ```

• 查看包详情  

  ```bash
  flutter pub deps
  ```

• 发布包到 pub.dev  

  ```bash
  flutter pub publish
  ```

---

通过熟练掌握这些命令和包管理操作，可以显著提升 Flutter 开发效率。遇到问题时，可通过 `flutter --help` 查看详细帮助文档。