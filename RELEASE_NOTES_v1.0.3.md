# 迈腾AI字典翻译 v1.0.3

本次为**图标与版本号更新**，功能与词库、模型内容相对 v1.0.0 不变。

**📦 下载**：[MTAIDict-1.0.3.dmg](https://github.com/sunyazhou13/MTAIDict-Releases/releases/latest/download/MTAIDict-1.0.3.dmg)

| 项 | 值 |
|---|---|
| 版本 | 1.0.3 (build 2) |
| 系统要求 | macOS 14.0 (Sonoma) 或更高，Universal 2（arm64 + x86_64） |
| 文件大小 | 1,973,106,176 B ≈ 1.84 GiB |
| SHA-256 | `9d344a3a11f6b2a07e2f64c1c126edc0577875bfba3f08effd6c1c1b1f5154b0` |
| 签名 | ad-hoc（未签名公证，首次打开需手动放行） |

## 🎨 本次更新

- **全新应用图标**：从「黑底金环 + 打开的书页」换成 **金边方框 + 深色皮革底 +
  金色字母 A + 蓝色斜块**。替换了 `Assets.xcassets/AppIcon.appiconset` 的全部 10 个尺寸切片
  （16/32/128/256/512 及各自 @2x），由 `actool` 重新编译为 `Assets.car` 与 `AppIcon.icns`。
  Dock、Finder、菜单栏「关于」面板以及 App 内侧栏品牌标识（都读 `NSApplication.shared.applicationIconImage`）
  会自动显示新图。
- **版本号**：营销版本 `1.0.0 → 1.0.3`，构建号 `1 → 2`；「关于」面板版本行同步显示 `1.0.3 (2)`。
- 其余功能、词库与内置模型均与 v1.0.0 一致；完整功能说明见
  [v1.0.0 Release](https://github.com/sunyazhou13/MTAIDict-Releases/releases/tag/v1.0.0) 与仓库 README。

> 若升级后 Dock 仍显示旧图标，是 LaunchServices 缓存未刷新，可运行源码工程的
> `bash Tools/refresh-icon-cache.sh /Applications/MTAIDict.app`，或注销/重启一次 Finder。

## ⚠️ 安装与提示

1. 打开 DMG，把「迈腾AI字典翻译」拖进「应用程序」覆盖旧版。
2. 首次打开若被 Gatekeeper 拦截：右键「打开」或「系统设置 → 隐私与安全性 → 仍要打开」，
   或 `xattr -cr /Applications/MTAIDict.app`。这是 ad-hoc 签名的分发限制，不是文件损坏。
3. 校验：`shasum -a 256 MTAIDict-1.0.3.dmg`，应得到上表 SHA-256。

已知限制（本地神经 TTS 未接入、QA 发版门禁未过、可选在线翻译兜底、部分词库许可待确认）
与 v1.0.0 相同，详见 [README「已知限制」](https://github.com/sunyazhou13/MTAIDict-Releases#已知限制)。

---

© 2026 sunyazhou.com
