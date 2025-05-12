---
title: FlutterLogin
date: 2025-05-12 20:21:30
tags:
---

在 Flutter 项目中实现 Google 和 Apple 登录

要在 Flutter 应用中集成 Google 和 Apple 登录功能，可以按照以下步骤操作：

# 准备工作

## 添加依赖项
在 `pubspec.yaml` 中添加必要的依赖：

```yaml
dependencies:
  firebase_auth: ^4.11.3
  google_sign_in: ^6.1.5
  sign_in_with_apple: ^5.0.0
  flutter_facebook_auth: ^4.6.3 # 可选，如果需要Facebook登录
```

运行 `flutter pub get` 安装依赖。

# 配置 Firebase（用于 Google 登录）

## 创建 Firebase 项目
1. 访问 [Firebase 控制台](https://console.firebase.google.com/)
2. 创建新项目并添加 Android/iOS 应用
3. 下载 `google-services.json` (Android) 和 `GoogleService-Info.plist` (iOS)
4. 将这些文件放在项目对应目录中

## 配置 Firebase Authentication
在 Firebase 控制台中启用 Google 和 Apple 登录方法。

# 实现 Google 登录

## Android 配置
在 `android/app/build.gradle` 中添加：

```gradle
dependencies {
    implementation platform('com.google.firebase:firebase-bom:32.0.0')
    implementation 'com.google.firebase:firebase-auth'
    implementation 'com.google.android.gms:play-services-auth:20.6.0'
}
```

## iOS 配置
在 `ios/Runner/Info.plist` 中添加：

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>REVERSED_CLIENT_ID</string> <!-- 从GoogleService-Info.plist获取 -->
        </array>
    </dict>
</array>
```

## 实现登录逻辑

```dart
import 'package:firebase_auth/firebase_auth.dart';
import 'package:google_sign_in/google_sign_in.dart';

Future<UserCredential?> signInWithGoogle() async {
  try {
    // 触发Google登录流程
    final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();
    
    if (googleUser == null) return null;
    
    // 获取认证详情
    final GoogleSignInAuthentication googleAuth = await googleUser.authentication;
    
    // 创建Firebase凭证
    final OAuthCredential credential = GoogleAuthProvider.credential(
      accessToken: googleAuth.accessToken,
      idToken: googleAuth.idToken,
    );
    
    // 使用Firebase登录
    return await FirebaseAuth.instance.signInWithCredential(credential);
  } catch (e) {
    print('Google登录错误: $e');
    return null;
  }
}
```

# 实现 Apple 登录

## iOS 配置
1. 在 Xcode 中启用 "Sign In with Apple" 能力
2. 在开发者账户中配置 Apple ID 登录

## Android 配置（可选）
从 Android 12 开始支持 Apple 登录，需要配置 `sign_in_with_apple` 插件。

## 实现登录逻辑

```dart
import 'package:firebase_auth/firebase_auth.dart';
import 'package:sign_in_with_apple/sign_in_with_apple.dart';

Future<UserCredential?> signInWithApple() async {
  try {
    // 触发Apple登录流程
    final credential = await SignInWithApple.getAppleIDCredential(
      scopes: [
        AppleIDAuthorizationScopes.email,
        AppleIDAuthorizationScopes.fullName,
      ],
    );
    
    // 创建OAuth提供者
    final oauthProvider = OAuthProvider('apple.com');
    
    // 创建Firebase凭证
    final firebaseCredential = oauthProvider.credential(
      idToken: credential.identityToken,
      accessToken: credential.authorizationCode,
    );
    
    // 使用Firebase登录
    return await FirebaseAuth.instance.signInWithCredential(firebaseCredential);
  } catch (e) {
    print('Apple登录错误: $e');
    return null;
  }
}
```

# 创建统一的登录界面

```dart
import 'package:flutter/material.dart';

class LoginScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () async {
                final user = await signInWithGoogle();
                if (user != null) {
                  // 登录成功，导航到主页
                  Navigator.pushReplacementNamed(context, '/home');
                }
              },
              child: Text('使用Google登录'),
            ),
            SizedBox(height: 20),
            SignInWithAppleButton(
              onPressed: () async {
                final user = await signInWithApple();
                if (user != null) {
                  // 登录成功，导航到主页
                  Navigator.pushReplacementNamed(context, '/home');
                }
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

# 处理用户状态

```dart
// 在主widget中监听认证状态
StreamBuilder<User?>(
  stream: FirebaseAuth.instance.authStateChanges(),
  builder: (context, snapshot) {
    if (snapshot.hasData) {
      return HomePage(); // 用户已登录
    }
    return LoginScreen(); // 用户未登录
  },
)
```

# 用户注销

```dart
Future<void> signOut() async {
  await FirebaseAuth.instance.signOut();
  await GoogleSignIn().signOut(); // 仅Google登录需要
}
```

注意事项

1. Apple 登录要求：如果应用支持 Apple 登录，且应用在 App Store 上架，所有其他社交登录选项都必须提供 Apple 登录选项
2. 测试环境：在开发阶段，确保在设备上测试，模拟器可能无法正常工作
3. 隐私政策：确保应用有隐私政策链接，特别是使用社交登录时
4. 用户数据：处理用户可能拒绝提供电子邮件或姓名的情况

进阶功能

1. 链接多个认证提供者：使用 `linkWithCredential()` 方法
2. 错误处理：完善各种错误情况的处理
3. 分析集成：添加分析跟踪登录事件
4. 深度链接：处理登录后的重定向

以上实现提供了完整的 Google 和 Apple 登录集成方案，可以根据您的具体需求进行调整。