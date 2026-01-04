# Repository 模式

## 设计原则

- **接口定义在 Domain 层**: Repository 接口定义在 `domain/repositories/`
- **实现在 Data 层**: Repository 实现放在 `data/repositories/`
- **依赖注入**: 通过 Riverpod 注入依赖

## 接口定义（Domain 层）

```dart
// lib/features/editor/domain/repositories/todo_repository.dart
abstract class TodoRepository {
  Future<List<Todo>> getAll();
  Future<Todo> getById(String id);
  Future<void> create(Todo todo);
  Future<void> update(Todo todo);
  Future<void> delete(String id);
}
```

## 实现（Data 层）

```dart
// lib/features/editor/data/repositories/todo_repository_impl.dart
@riverpod
TodoRepository todoRepository(TodoRepositoryRef ref) {
  return TodoRepositoryImpl(
    isar: ref.watch(isarProvider),
    api: ref.watch(apiClientProvider),
  );
}

class TodoRepositoryImpl implements TodoRepository {
  final Isar isar;
  final ApiClient api;
  // 实现接口方法
}
```

## 核心领域策略层

### 设计原则
- **纯函数优先**: 所有策略类应该是无副作用的纯函数或纯类
- **可移植性**: 核心逻辑应该可以在任何 Dart 环境中运行（不依赖 Flutter）
- **可测试性**: 不依赖外部服务，便于单元测试

### 同步服务 (SyncService)

**职责**:
- 调度所有同步任务
- 管理分布式锁（防止多实例并发）
- 实现双模同步（增量 + 全量）

**实现位置**: `lib/core/sync/sync_service.dart`

**冲突解决策略**:
- 简单字段: Last-Write-Wins (LWW)
- 复杂字段: CRDT 或三路合并 (Three-Way Merge)
- 使用 `ShadowMap` 追踪上次同步状态

## 测试要求

- 所有策略类必须有对应的单元测试
- 测试文件命名：`*_test.dart`
- 覆盖率要求：90%+

详见: [架构分层原则](./架构分层原则.md) | [数据同步](../data/数据同步.md)

