# ADR-001: 为什么选择 Riverpod 而不是 Bloc

## 状态

已采用

## 背景

Flutter 项目需要选择一个状态管理方案。状态管理是 Flutter 应用架构的核心部分，需要支持：
- 分层架构（Clean Architecture）
- 依赖注入
- 编译时类型安全
- 易于测试

## 考虑的选项

### 1. Riverpod (Generator)

**优点**:
- 编译时安全，类型错误在编译期发现
- 无 Context 依赖，可在任何地方访问 Provider
- 完美契合分层架构，Provider 可以按层级组织
- 代码生成器（@riverpod）自动生成代码，减少样板代码
- 强大的依赖注入能力
- 内置的测试支持

**缺点**:
- 学习曲线相对较陡
- 需要理解 Riverpod 特有的概念（Ref、Provider 类型等）
- 代码生成需要运行 build_runner

### 2. Bloc

**优点**:
- 清晰的单向数据流（Event -> State）
- 强大的测试能力
- 良好的文档和社区支持
- 适合复杂的状态逻辑

**缺点**:
- 样板代码较多（Event、State、Bloc 类）
- 需要 Context 访问（BlocProvider.of）
- 状态管理逻辑可能变得复杂
- 与分层架构的集成相对复杂

### 3. Provider

**优点**:
- Flutter 官方推荐
- 简单易学
- 轻量级

**缺点**:
- 类型安全较弱（运行时错误）
- 需要 Context，不适合分层架构
- 缺少代码生成，样板代码多

## 决策

选择 **Riverpod (Generator)** 作为状态管理方案。

## 理由

1. **编译时安全**: Riverpod 的编译时类型检查可以在开发阶段发现错误，而不是运行时。这对于大型项目至关重要。

2. **无 Context 依赖**: Riverpod 的 Provider 可以在任何地方访问，不依赖于 Widget 树，完美契合 Clean Architecture 的分层设计。

3. **代码生成**: `@riverpod` 注解和代码生成器大大减少了样板代码，提高了开发效率和代码一致性。

4. **分层架构支持**: Provider 可以按层级组织（core、data、presentation），与我们的架构设计完美匹配。

5. **依赖注入**: Riverpod 的依赖注入机制强大且灵活，支持测试时的 Mock 替换。

## 后果

### 正面影响

- 代码类型安全，减少运行时错误
- 更好的代码组织和可维护性
- 减少样板代码，提高开发效率
- 更容易进行单元测试

### 负面影响和风险

- 团队需要学习 Riverpod 的特有概念（Ref、Provider 类型、代码生成流程）
- 新成员可能需要时间适应代码生成的工作流
- Riverpod 相对于 Bloc 的生态系统稍小，但足够成熟

### 注意事项

- 确保团队了解 Riverpod 的代码生成流程（build_runner）
- 建立 Riverpod Provider 的组织规范（参考 [技术选型](../principles/framework/技术选型.md)）
- 在 CI/CD 中集成代码生成检查

---

**相关文档**:
- [技术选型](../principles/framework/技术选型.md)
- [架构分层原则](../principles/framework/架构分层原则.md)

