# Flutter AI 辅助开发 Prompt 模板

**核心原则：架构感知 (Architecture-Aware) · 垂直生成 (Vertical Generation) · 自动化检查 (Automated Review)**

> **关联文档**: 本指南与 [AI 辅助开发通用规范指南](./AI%20辅助开发通用规范指南.md) 配合使用，提供 Flutter 架构特定的 Prompt 模板。

## 垂直生成指令

**完整生成指令** (Entity -> Repository -> Provider -> Widget):
```
Generate a complete feature following Flutter Clean Architecture:

1. Entity: `lib/features/{feature}/domain/entities/{entity}.dart` - @freezed, JSON serialization
2. Repository Interface: `lib/features/{feature}/domain/repositories/{entity}_repository.dart` - abstract class, CRUD methods
3. Repository Impl: `lib/features/{feature}/data/repositories/{entity}_repository_impl.dart` - Isar (local) + Supabase (remote), offline-first
4. Provider: `lib/features/{feature}/presentation/providers/{entity}_provider.dart` - @riverpod, optimistic update
5. Widget: `lib/features/{feature}/presentation/widgets/{entity}_list.dart` - ref.watch(), AsyncValue handling

Constraints: Domain (NO Flutter/Riverpod) -> Data (Domain+Core) -> Presentation (Domain), optimistic update, AppException
```

## 特定场景 Prompt

**Repository 实现**:
```
Generate Repository for `{Entity}`: Implement interface using Isar (offline-first) + Supabase, convert errors to AppException, follow `core/exceptions/app_exceptions.dart` pattern.
```

**Provider 实现**:
```
Generate Provider for `{Entity}`: @riverpod, optimistic update (save state -> update UI -> submit -> rollback on error), use SyncService, handle AppException types.
```

## Code Review 自动化检查

**检查规则**: Domain 层导入检查、依赖方向、乐观更新、错误处理、Repository 模式

**Prompt 模板**:
```
Review Flutter code: Check domain imports (NO Flutter/Riverpod), dependency direction (Presentation->Domain->Data->Core), optimistic update, AppException usage, Repository pattern. Report violations with file paths.
```

## 与通用规范的关联

本指南遵循 [AI 辅助开发通用规范指南](./AI%20辅助开发通用规范指南.md) 的核心理念：

- **模块化**: Flutter 特定规则作为"插件"扩展通用规范
- **关注点分离**: 架构规则与业务逻辑分离
- **工具强制**: 通过 Lint 规则和 AI 检查双重保障

详见: [架构分层原则](../Flutter/principles/framework/架构分层原则.md) | [Repository模式](../Flutter/principles/framework/Repository模式.md)

