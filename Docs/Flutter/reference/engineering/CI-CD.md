# CI/CD

**核心原则：自动化 (Automation) · 多平台构建 (Multi-Platform Build)**

## GitHub Actions 配置

**实现位置**: `.github/workflows/ci.yml`

**工作流**:
1. **Analyze**: 运行 `flutter analyze` 和 `flutter test`
2. **Build**: 多平台构建（Windows、macOS）

**关键步骤**:
- 安装 Flutter SDK
- 运行 `flutter pub get`
- 运行 `dart run build_runner build` 生成代码
- 运行 `flutter analyze` 检查
- 运行 `flutter test` 测试
- 运行 `flutter build` 构建

## 构建产物

**Windows**: `.exe` 可执行文件
**macOS**: `.dmg` 安装包或 `.app` 应用

**MVP 阶段**: 可暂缓自动签名，优先保证构建产物可测试。

## 环境配置

**项目初始化**:
1. 安装 Flutter SDK (3.0+)
2. 运行 `flutter pub get`
3. 运行 `dart run build_runner build` 生成代码
4. 运行 `flutter analyze` 检查配置

详见: [构建工作流](./构建工作流.md) | [架构守护](./架构守护.md)


