# 迈腾AI字典翻译 v1.0.0

首个公开构建。macOS 离线优先的英汉词典与翻译工具，词库、翻译模型与语音识别模型全部随包，
**装完即可全离线使用**，不需要账号、不需要首启下载。

**📦 下载**：[MTAIDict-1.0.0.dmg](https://github.com/sunyazhou13/MTAIDict-Releases/releases/latest/download/MTAIDict-1.0.0.dmg)

| 项 | 值 |
|---|---|
| 版本 | 1.0.0 (build 1) |
| 系统要求 | macOS 14.0 (Sonoma) 或更高，Universal 2（arm64 + x86_64） |
| 文件大小 | 1,974,277,632 B ≈ 1.84 GiB |
| SHA-256 | `719fba8a5d9acf593cc0a0b96fe0c28e5d6e67a6bd5a94fee0ff5df86f2b5cab` |
| 签名 | ad-hoc（未签名公证，首次打开需手动放行） |

## ✨ 本版本内容

### 查词
- **英→中 339 万词条**：音标、词性、多义项、英文释义、发音与例句；主库 342 MB，WCDB + SQLCipher 加密。
- **中→英 12.5 万词条**（含繁体别名约 20 万行）+ 31 万条中文释义（汉字 16,142 / 词语 264,434 / 成语 30,895）。
- **中文反查索引**：紧凑 bigram 倒排，把 `LIKE '%…%'` 全表扫描的 **12–13 秒压到 55–250 ms**；
  索引在首启时于 `Application Support/MTAIDict/mtdict_fts.sqlite` 构建（约 110 MB，13.5 秒）。
- **AI 讲解 / 例句**：本地模型生成；内置模型是翻译专用，遇到讲解类格式指令会立即 EOS，
  此时自动降级为「本地词库整理」并在界面上**如实标注来源**，不弹错误、不把词库结果冒充模型生成。

### 翻译与语音
- **句子翻译**：整句中英互译，方向按是否含 CJK 自动判定，走本地 GGUF。提示词按模型家族分流
  （混元走直接指令、通用对话模型走 few-shot），避免通用模型把「翻译这句」当聊天回答。
- **文件翻译**：`.md` / `.txt` / `.srt` / `.csv` 整篇翻译，逐行分块，代码块 / front matter /
  图片 / HTML / 表格分隔 / 行内 code / 链接 URL 原样保留；未拿到译文的行退回原文，绝不丢内容。
- **语音输入**：默认 **whisper.cpp v1.9.2 + 随包 ggml-small.bin（465 MB）**，
  `AVAudioEngine` → 16 kHz 单声道 float32 → Metal 推理 → 繁转简，全程离线；
  可勾选「改用系统识别」（`SFSpeechRecognizer`，联网、流式，等 `isFinal` 再翻译）。

### 面向开发者
- **研发命名（含 AI 命名）**：中文词或**一整句** → 5 种命名风格 × 13 种语言保留字避让 →
  变量 / 类 / 常量三档候选 + 一键复制。本地词库毫秒级；整句点「AI 命名」由本地模型压缩语义。
  两段结果分区展示并各自标注来源，AI 失败时词库结果原样保留。
  本次还修掉一个实测暴露的语义反转：译文 `"has not paid"` 的 `not` 曾被停用词丢掉，
  把「未支付」变成「已支付」，现折叠为 `unpaid`（`ordersUserUnpaidDays`）。
- 共 9 个页面：查词、研发命名、句子翻译、文件翻译、发音、历史、生词本、词库、设置。

### 工程侧（提升可维护性，不改变用户可见功能）
- 全工程迁移到 **Swift 6 语言模式**，47 个 Swift 文件全量 `-swift-version 6`，
  clean build 0 条源码告警、单测 125 用例 / 818 断言全绿。
- 告警从 117 条收敛到 1 条（仅为 `appintentsmetadataprocessor` 的环境提示）。
- 大文件迁 **Git LFS**（`.gguf` / `.bin` / `.sqlite`），源码仓保持轻量。
- 新增 `build.sh` 打包脚本：LFS 资源体检、ad-hoc 重签、UDZO DMG、`--release` 证书守卫。
- 版本号统一到 1.0.0，「关于」面板与菜单栏应用名统一为「迈腾AI字典翻译」。

## 🖥 系统要求与隐私

- macOS 14.0+，Universal 2，App Sandbox + Hardened Runtime 已开启。
- 查词 / 翻译 / 语音识别默认设备端完成，无账号、无遥测、无广告 SDK（`NSPrivacyTracking = false`）。
- 数据只落在本机沙盒容器；词库 WCDB + SQLCipher，设置 / 历史 / 生词本走 MMKV。
- ⚠️ **并非完全不联网**：查词有可选在线翻译兜底（Edge / MyMemory / dictionaryapi.dev），
  语音可切换系统识别（联网）。**MyMemory 会把查询文本纳入其公共记忆库**，在意隐私请勿启用。

## 📊 体积构成

| 内容 | 体积 |
|---|---|
| 内置翻译模型 Hy-MT2-1.8B-Q4_K_M.gguf | 1,133,080,448 B ≈ 1.06 GiB |
| 离线语音模型 ggml-small.bin | 487,601,967 B ≈ 465 MiB |
| 英汉主库 mtdict.sqlite | 358,793,216 B ≈ 342 MiB |
| 中文释义 xinhua.sqlite | 69,672,960 B ≈ 66 MiB |
| 中英词典 zhen.sqlite | 20,533,248 B ≈ 20 MiB |
| App 合计 | ≈ 2.0 GB |
| **DMG（UDZO 压缩）** | **1,974,277,632 B ≈ 1.84 GiB** |

> DMG 单个 asset 必须 < 2 GiB（GitHub 官方限制），本版本约 1.84 GiB，**只剩约 173 MB 余量**。
> 后续若模型或词库继续增大，需要考虑拆分资源或改首启下载。

## ⚠️ 安装与已知限制

1. 打开 DMG，把「迈腾AI字典翻译」拖进「应用程序」。
2. 首次打开若被 Gatekeeper 拦截：右键「打开」或「系统设置 → 隐私与安全性 → 仍要打开」，
   或 `xattr -cr /Applications/MTAIDict.app`。这是 ad-hoc 签名的分发限制，不是文件损坏。

- **本地神经 TTS 未接入**：当前发音用系统音色（本机实测 59 个中英音色 quality 全是 compact，听感偏机械）；
  sherpa-onnx 方案已完成选型与离屏实测，未随本版本发布。
- **未做 Developer ID 签名与公证**；上架决策（1.4 GB 模型是否继续随包）未定。
- **QA 发版门禁未通过**（测试执行率 42.5%、代码覆盖率未插桩），本产物为可运行构建而非正式上架版本。
- **许可待确认**：`chinese-xinhua` 标注「个人使用；上架需自行确认授权」；CC-CEDICT 与
  `ggml-small` 权重在工程内尚未补书面许可标注。上架前需完成 `TC-SEC-010` 许可审查。
- 词形变化 / 例句数据有缺口（ECDICT 分片无 `exchange`）；`chinese-xinhua` 词语表偏古汉语，
  现代多字词走逐字释义。麦克风实时输入与热键链路未真机实测。
- 权限清单中 Pictures / Music / Movies 读写并非词典必需，后续会收敛。

## 🙏 数据来源与致谢

ECDICT（MIT）· Wiktionary / kaikki（CC BY-SA）· `pwxcoo/chinese-xinhua` · CC-CEDICT ·
腾讯混元 Hy-MT2-1.8B（Apache-2.0）· llama.cpp（MIT）· whisper.cpp v1.9.2 ·
WCDB 2.1.8 / SQLCipher 1.4.6 / MMKV 2.4.3 · SwiftUI。

---

© 2026 sunyazhou.com
