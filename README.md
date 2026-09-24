# flutter_template

Flutter 工程模板：lint、format、test、Conventional Commits 与 CI 已接入。后续 FluentAnyLang 等应用可基于此仓库继续开发。

## 前置条件

- [Flutter SDK](https://docs.flutter.dev/get-started/install)（稳定版）
- [Node.js](https://nodejs.org/)（仅用于 Git hooks：`commitlint`、`lefthook`）
- [pnpm](https://pnpm.io/) 11.9.0（`package.json` 中 `packageManager` 锁定；可用 `corepack enable` 自动对齐）

本仓库 **仅允许 pnpm** 安装 Node 工具依赖（`preinstall` + `only-allow`）。pnpm 11 在 [`pnpm-workspace.yaml`](pnpm-workspace.yaml) 中通过 `allowBuilds` 允许 `lefthook` 安装脚本。

## 常用命令

| 命令 | 说明 |
|------|------|
| `flutter pub get` | 拉取 Dart 依赖 |
| `pnpm install` | 安装 hooks 依赖并执行 `lefthook install` |
| `pnpm run format` | 格式化 `lib/`、`test/` 等 Dart 代码 |
| `pnpm run format:check` | 检查格式（CI 同款，不写入） |
| `pnpm run analyze` | `flutter analyze --fatal-infos` |
| `pnpm run test` | `flutter test` |
| `pnpm run verify` | format 检查 + analyze + test |

等价 Flutter/Dart 命令：`dart format .`、`dart analyze` / `flutter analyze`、`flutter test`。

## Lint

[`analysis_options.yaml`](analysis_options.yaml) 使用官方 [`flutter_lints`](https://pub.dev/packages/flutter_lints)，并启用 `prefer_single_quotes`。

## Git hooks（Lefthook）

安装依赖后 hooks 自动注册：

- **pre-commit**：对已暂存的 `*.dart` 执行 `dart format`，并跑 `flutter analyze --fatal-infos`
- **commit-msg**：[`commitlint`](commitlint.config.cjs) 校验 Conventional Commits（如 `feat: …`、`fix: …`）
- **pre-push**：`flutter test`

跳过单次 hook：`LEFTHOOK=0 git commit …`

## CI

推送或 PR 到 `main` / `master` 时，[`.github/workflows/ci.yml`](.github/workflows/ci.yml) 在 Ubuntu 上执行 format 检查、analyze、test。

## 编辑器

推荐安装 [Dart](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code) 与 [Flutter](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter) 扩展；仓库内 [`.vscode/settings.json`](.vscode/settings.json) 已开启 Dart 保存时格式化。

## 平台工具链

本模板不强制安装 Android Studio / Visual Studio。仅做业务开发与 Web 调试时，`flutter doctor` 中对应项未就绪也可正常跑 analyze 与 test；需要打包 Android / Windows 桌面时再按 [Flutter 文档](https://docs.flutter.dev/) 补齐即可。
