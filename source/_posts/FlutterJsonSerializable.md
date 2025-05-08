---
title: FlutterJsonSerializable
date: 2025-05-08 17:27:53
tags: [Json,Flutter]
categories: [Flutter]
---

使用 json_serializable 在 Flutter/Dart 中进行 JSON 序列化

`json_serializable` 是 Dart 中一个强大的代码生成库，用于自动生成 JSON 序列化和反序列化的代码。以下是详细的使用指南：

# 添加依赖

首先，在 `pubspec.yaml` 中添加必要的依赖：

```yaml
dependencies:
  json_annotation: ^4.8.1

dev_dependencies:
  build_runner: ^2.4.6
  json_serializable: ^6.7.1
```
使用命令添加：
```bash
flutter pub add json_annotation
flutter pub add build_runner --dev
flutter pub add json_serializable --dev
```

然后运行：
```bash
flutter pub get
```

# 创建可序列化的模型类

创建一个 Dart 类并使用 `@JsonSerializable()` 注解：

```dart
import 'package:json_annotation/json_annotation.dart';

part 'user.g.dart'; // 生成的代码文件

@JsonSerializable()
class User {
  final String name;
  final String email;
  @JsonKey(name: 'registered_at') // JSON字段名映射
  final DateTime registeredAt;
  @JsonKey(includeIfNull: false) // 为null时不包含此字段
  final int? age;
  
  User({
    required this.name,
    required this.email,
    required this.registeredAt,
    this.age,
  });

  // 反序列化工厂方法
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  
  // 序列化方法
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

# 生成序列化代码

运行以下命令生成 `.g.dart` 文件：

```bash
flutter pub run build_runner build
```

或者使用 watch 模式（自动检测文件变化并重新生成）：

```bash
flutter pub run build_runner watch
```

# 使用生成的代码

从 JSON 反序列化

```dart
final jsonString = '''
{
  "name": "John Doe",
  "email": "john@example.com",
  "registered_at": "2023-01-01T00:00:00Z",
  "age": 30
}
''';

final user = User.fromJson(jsonDecode(jsonString));
print(user.name); // 输出: John Doe
```

序列化为 JSON

```dart
final user = User(
  name: 'Jane Smith',
  email: 'jane@example.com',
  registeredAt: DateTime.now(),
  age: null,
);

final json = user.toJson();
print(jsonEncode(json));
// 输出: {"name":"Jane Smith","email":"jane@example.com","registered_at":"2023-01-01T00:00:00Z"}
// 注意age字段被排除，因为设置了includeIfNull: false且值为null
```

# 高级配置选项

自定义 JSON 键名

```dart
@JsonSerializable()
class Product {
  @JsonKey(name: 'product_id')
  final String id;
  @JsonKey(name: 'product_name')
  final String name;
  
  Product({required this.id, required this.name});
}
```

处理枚举

```dart
enum UserRole { admin, moderator, user }

@JsonSerializable()
class User {
  final UserRole role;
  
  User({required this.role});
  
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}

// 在main.dart或单独文件中添加：
const userRoleEnumMap = {
  UserRole.admin: 'admin',
  UserRole.moderator: 'moderator',
  UserRole.user: 'user',
};
```

自定义序列化逻辑

```dart
@JsonSerializable()
class CustomObject {
  final DateTime date;
  
  CustomObject({required this.date});
  
  factory CustomObject.fromJson(Map<String, dynamic> json) => 
      CustomObject(
        date: DateTime.parse(json['date'] as String),
      );
  
  Map<String, dynamic> toJson() => {
    'date': date.toIso8601String(),
  };
}
```

# 处理嵌套对象

```dart
@JsonSerializable()
class Address {
  final String street;
  final String city;
  
  Address({required this.street, required this.city});
  
  factory Address.fromJson(Map<String, dynamic> json) => _$AddressFromJson(json);
  Map<String, dynamic> toJson() => _$AddressToJson(this);
}

@JsonSerializable()
class User {
  final String name;
  final Address address;
  
  User({required this.name, required this.address});
  
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

# 常见问题解决

清理旧的生成文件

```bash
flutter pub run build_runner clean
```

强制重新生成

```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

处理泛型类

```dart
@JsonSerializable(genericArgumentFactories: true)
class Response<T> {
  final T data;
  
  Response({required this.data});
  
  factory Response.fromJson(
    Map<String, dynamic> json,
    T Function(Object? json) fromJsonT,
  ) => _$ResponseFromJson(json, fromJsonT);
  
  Map<String, dynamic> toJson(Object? Function(T value) toJsonT) =>
      _$ResponseToJson(this, toJsonT);
}
```

# 与 Flutter 结合的最佳实践

## 1. API 响应处理：

```dart
Future<User> fetchUser() async {
  final response = await http.get(Uri.parse('https://api.example.com/user'));
  if (response.statusCode == 200) {
    return User.fromJson(jsonDecode(response.body));
  } else {
    throw Exception('Failed to load user');
  }
}
```

## 2. 本地存储：

```dart
Future<void> saveUser(User user) async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString('user', jsonEncode(user.toJson()));
}

Future<User?> loadUser() async {
  final prefs = await SharedPreferences.getInstance();
  final userJson = prefs.getString('user');
  return userJson != null ? User.fromJson(jsonDecode(userJson)) : null;
}
```

通过 `json_serializable`，你可以大大简化 Dart/Flutter 项目中的 JSON 序列化工作，同时保持类型安全和代码的可维护性。

# 插件
Android Studio Plugin:`DartMappable​(Flutter Json To Freezed JsonSerializable Compat)​`

通过插件可以快速生成json_serializable代码