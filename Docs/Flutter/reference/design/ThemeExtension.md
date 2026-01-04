# ThemeExtension

**核心原则：单一真理源 (SSOT) · 语义化 (Semantic) · 类型安全 (Type-Safe)**

> **单一真理源声明**: 所有设计值定义在 `lib/core/theme/`，文档仅说明语义用途。

## ThemeExtension 系统

**规则**: 不要直接使用 `Colors.blue`。使用 `ThemeExtension` 扩展 Flutter 原生主题，实现语义化 Token。

**实现位置**: `lib/core/theme/app_colors.dart`

```dart
@immutable
class AppColors extends ThemeExtension<AppColors> {
  final Color bgCanvas;      // 对应 --c-bg
  final Color textPrimary;   // 对应 --c-content
  final Color action;        // 对应 --c-action
  final Color glass;          // 对应 --c-glass
  final Color light;          // 对应 --c-light
  final Color dark;           // 对应 --c-dark
  
  const AppColors({...});
  
  @override
  ThemeExtension<AppColors> copyWith({...}) { ... }
  @override
  ThemeExtension<AppColors> lerp(ThemeExtension<AppColors>? other, double t) { ... }
}
```

## 使用 ThemeExtension

**规则**: 在 Widget 中通过 `Theme.of(context).extension<AppColors>()` 访问颜色。

```dart
Container(
  color: Theme.of(context).extension<AppColors>()!.bgCanvas,
  child: Text(
    'Hello',
    style: TextStyle(
      color: Theme.of(context).extension<AppColors>()!.textPrimary,
    ),
  ),
)
```

## 文本样式扩展

**实现位置**: `lib/core/theme/app_text_styles.dart`

```dart
@immutable
class AppTextStyles extends ThemeExtension<AppTextStyles> {
  final TextStyle heading1;
  final TextStyle heading2;
  final TextStyle body;
  final TextStyle caption;
  // 实现 copyWith 和 lerp...
}
```

## 主题系统

**实现位置**: `lib/core/theme/app_theme.dart`

支持动态主题切换，通过 `ThemeMode` 实现。

详见: [设计常量](./设计常量.md) | [响应式布局](./响应式布局.md)

