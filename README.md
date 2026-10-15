# Fushi 个人补丁：提取文本自动复制

本仓库保存 Windows Fushi 的个人功能补丁，供以后迁移到官方新版。上游项目：[hajisensai/Fushi](https://github.com/hajisensai/Fushi)。本仓库为独立私有仓库，上游保持只读，不向上游推送或创建 PR。

## 当前补丁

- 原版：Fushi **2.9.1**，已安装程序版本 **2.9.1+16909**。
- 对应源码：`689f44456cd224641e916da4ef5bf38438771b29`。
- 补丁：[patches/0001-windows-gal-text-auto-copy.patch](patches/0001-windows-gal-text-auto-copy.patch)。
- 文件清单、基线和校验值：[manifest.json](manifest.json)。
- 开关位置：**设置 → 游戏 → 提取文本自动复制到剪贴板**。
- 默认关闭；偏好键：`gal_text_auto_copy_enabled`；设置项 ID：`game.text_auto_copy`。

开启后，当前选定文本流的新台词到达时自动复制到 Windows 剪贴板。开关在重启后保留，不需要一直打开工作台。启用时不重放历史；未选中的文本线程、音频状态更新和收藏等通知不会触发重复复制。逐步补全的同一行文本会复制更新后的内容。异步写入按顺序执行，禁用或退出会取消尚未开始的写入，单次剪贴板写入失败不会阻断后续台词。

## 仓库内容

| 文件 | 用途 |
| --- | --- |
| `patches/0001-windows-gal-text-auto-copy.patch` | 完整功能差异，包含新增源文件、测试及生成的翻译代码 |
| `manifest.json` | 上游精确版本、补丁 SHA-256、27 个变更文件的前后 Git blob 与修改后 SHA-256 |
| `docs/UPGRADE.md` | 给后续维护者与 AI 的新版迁移流程 |
| `docs/BUILD_AND_SERVICES.md` | 本次构建经验、在线服务保留要求与安装边界 |
| `docs/VERIFICATION.md` | 已完成的测试与未验证范围 |
| `AGENTS.md` | 后续 AI 的操作边界 |
| `LICENSE` | 从记录的上游基线复制的许可证 |

本仓库只存补丁及维护资料。原版和改版二进制、恢复副本、Flutter SDK、依赖缓存、个人资料与实际在线服务配置均留在本机。

## 在原基线上应用

准备上游源码的独立工作副本，确认其为上面的精确提交。将下方 `<patch-file>` 替换成补丁的绝对路径：

```text
git remote set-url --push origin DISABLED-READONLY
git switch --detach 689f44456cd224641e916da4ef5bf38438771b29
git apply --check <patch-file>
git apply --3way --index <patch-file>
```

只在干净的工作副本中应用；不要覆盖已有改动。`git apply` 支持本仓库的普通 unified diff 补丁，不使用 `git am`。

应用后按上游构建说明准备依赖，再执行 [验证流程](docs/VERIFICATION.md)。**源码能够编译，不代表官方在线功能已经保留**；替换日常使用的软件前须完成 [在线服务检查](docs/BUILD_AND_SERVICES.md)。

## 官方出新版时

先读 [docs/UPGRADE.md](docs/UPGRADE.md)。先确认官方是否已经提供同类功能，再根据新版源码迁移这份小改动。迁移成功后更新补丁、基线、清单和测试证据。当前补丁对精确基线的还原已验证；对未来版本的兼容性需要重新验证。
