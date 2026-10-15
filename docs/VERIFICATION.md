# 验证记录

日期：2026-10-15。适用基线和文件清单见根目录 `manifest.json`。

## 功能实施时已完成

- 16 项新增行为测试通过：控制器 13 项、偏好保存 2 项、设置接线 1 项。
- 加上既有偏好键注册守卫后，定向测试共 19 项通过。
- 相关文本、队列、翻译回归共 44 项通过；设置覆盖守卫 2 项通过。
- 修改后的 8 个源文件和测试文件定向静态分析无问题；Git 空白差异检查通过。
- Windows 真实设置界面中存在新开关，点击后能保存至隔离数据库。
- 最终程序在预览目录以及日常安装目录中分别通过三次运行：开启时复制新文本、关闭时剪贴板不变、保存开启设置并重启后继续复制。测试使用本地 WebSocket 日文句子和实际 Windows 剪贴板，测试后恢复原剪贴板。
- 最终包的七项原版应用级在线配置精确一致，并核对了消费函数引用；原生组件保留，个人资料未迁移。

## 定向测试入口

按照对应版本的上游资源控制规范，通过 `tool/heavy.dart` 运行 Flutter 命令。完成依赖准备后，在源码的 `fushi/` 目录运行：

```text
dart tool/heavy.dart -- flutter test --no-pub test/models/preference_keys_guard_test.dart test/mining/gal_text_auto_copy_controller_test.dart test/models/gal_text_auto_copy_preferences_test.dart test/settings/gal_text_auto_copy_settings_test.dart
dart tool/heavy.dart -- flutter test --no-pub test/settings/settings_schema_coverage_test.dart
```

与功能相关的既有文本、队列及 i18n 测试也应按新版实际路径选取。上游测试入口、资源调度或生成工具变化时优先遵循新版规范。

## 上传归档时的验证

本次归档另行检查完整补丁可在精确基线应用，并逐个比对变更文件内容与清单。上传前扫描所有待提交文件，排除本机七项实际服务配置及程序、数据库和缓存。详细结果写入 `archive-verification.json`。

## 验证边界

上述实施记录来自本次本机执行，上传整理期间不重复运行未改变代码的全部测试。归档检查证明补丁完整性；不能代替未来版本的功能回归。未逐项重新登录在线服务，也未用真实运行中的 Galgame 作为本次新功能测试源。
