# 迈腾AI字典翻译

一款 macOS **离线优先**的英汉词典与翻译工具。查词、句子翻译、语音识别都跑在本机，
内置 339 万英汉词条、中文释义库与本地大模型，不依赖账号、无广告。

> 本仓库仅用于**分发构建产物与版本发布说明**；源码仓库暂不公开。
>
> 📌 **当前状态**：本目录为本地发布骨架，GitHub 仓库与 Release **尚未创建**，
> 下方下载链接待 `v1.0.0` Release 上传 `MTAIDict-1.0.0.dmg` 后生效。
> DMG 已在源码工程 `MTAIDict/dist/` 构建完成（SHA-256 见下表）。

## 下载

**[下载最新版 MTAIDict-1.0.0.dmg](https://github.com/sunyazhou13/MTAIDict-Releases/releases/latest/download/MTAIDict-1.0.0.dmg)**

也可以到 [Releases 页面](https://github.com/sunyazhou13/MTAIDict-Releases/releases) 查看所有版本与完整发布说明。

| 项 | 值 |
|---|---|
| 版本 | 1.0.0 (build 1) |
| 系统要求 | macOS 14.0 (Sonoma) 或更高 |
| 架构 | Universal 2（Apple Silicon `arm64` + Intel `x86_64`） |
| 文件大小 | 1,974,277,632 B ≈ 1.84 GiB |
| SHA-256 | `719fba8a5d9acf593cc0a0b96fe0c28e5d6e67a6bd5a94fee0ff5df86f2b5cab` |
| 签名 | ad-hoc（未经 Developer ID 签名与公证，见下方「安装」） |

> **关于体积**：安装包约 1.84 GiB，因为词库与推理模型全部随包分发 ——
> 339 万英汉词条（342 MB）、中文释义库（66 MB）、中英词典（20 MB）、
> 本地翻译模型 Hy-MT2-1.8B Q4_K_M（1.06 GB）与离线语音识别模型 ggml-small（465 MB）。
> 换来的是**装完即可全离线使用，首次启动不需要下载任何资源**。

## 安装

1. 打开 DMG，把「迈腾AI字典翻译」拖进「应用程序」。
2. 首次打开若被 Gatekeeper 拦截，任选一种方式处理：
   - 在「系统设置 → 隐私与安全性」里点「仍要打开」；
   - 或右键 App → 「打开」；
   - 或终端执行 `xattr -cr /Applications/MTAIDict.app` 后重新打开。
3. 首次使用「语音输入」时，系统会请求麦克风与语音识别权限。

> ⚠️ **关于签名**：当前版本为 **ad-hoc 签名**，未经 Developer ID 签名与 Apple 公证，
> Gatekeeper 可能提示「已损坏，无法打开」。这是分发方式限制，**不是文件损坏**。
> 用上面的任一方式放行即可，之后永久可用。

## 主要功能

### 查词
- **英→中**：339 万词条，含音标、词性、多义项、英文释义、发音与例句。
- **中→英**：12.5 万词条（含繁体别名），以及逐字释义与拼音。
- **中文反查**：紧凑 bigram 倒排索引，从「LIKE 全表扫描 12–13 秒」降到 **55–250 ms**，
  索引在首次启动时于 `Application Support/MTAIDict/` 生成（约 110 MB）。
- AI 讲解与例句由本地模型生成；翻译专用模型不擅长生成讲解时，会**降级为本地词库整理并如实标注来源**，不弹红字、不冒充模型输出。

### 翻译与语音
- **句子翻译**：整句中英互译，走本地 GGUF，方向自动判定。
- **文件翻译**：`.md` / `.txt` / `.srt` / `.csv` 整篇翻译，分块并保留代码块、链接、front matter 等格式。
- **语音输入**：默认 **whisper.cpp + 随包 ggml-small**，16 kHz 采集 → Metal 推理 → 繁转简，全程离线；
  可切换系统识别（联网、流式）。

### 面向开发者
- **研发命名**：输入中文词或**一整句**，生成 5 种命名风格（camel / Pascal / snake / kebab / SCREAMING_SNAKE）
  × 13 种编程语言的保留字避让，给出变量 / 类 / 常量三档候选并一键复制。
  本地词库毫秒级出结果；整句可点「AI 命名」交本地模型理解语义。
- 历史、生词本、词库信息、发音（拼音声韵母 + 英式 DJ 44 音标）、外观与语言设置等共 9 个页面。

### 隐私
- 查词、翻译、语音识别默认**全部设备端完成**，无账号、无遥测、无广告 SDK；
  `PrivacyInfo.xcprivacy` 声明 `NSPrivacyTracking = false`。
- 数据只落在本机沙盒容器 `~/Library/Containers/com.maiteng.mtdict/`，
  词库走 WCDB + SQLCipher 加密，设置 / 历史 / 生词本走 MMKV。
- ⚠️ **并非「完全不联网」**：查词有可选的在线翻译兜底（Edge / MyMemory / dictionaryapi.dev），
  语音输入可勾选「改用系统识别」（联网）。这些功能默认不启用或只在兜底时触发。
  其中 **MyMemory 会把查询文本纳入其公共记忆库**，在意隐私时请勿使用在线兜底。

## 数据来源与致谢

| 数据 / 组件 | 来源 |
|---|---|
| 英汉主词库 342 MB | ECDICT（MIT）、Wiktionary / kaikki（CC BY-SA） |
| 中文释义 66 MB | `pwxcoo/chinese-xinhua` |
| 中英词典 20 MB | CC-CEDICT |
| 翻译模型 | 腾讯混元 Hy-MT2-1.8B Q4_K_M（Apache-2.0，随包分发） |
| 推理 / 语音引擎 | llama.cpp（MIT）、whisper.cpp v1.9.2 |
| 存储 | WCDB 2.1.8、SQLCipher 1.4.6、MMKV 2.4.3（本地编译，BSD 系许可） |
| 界面 | SwiftUI |

> ⚠️ **许可待确认**：`chinese-xinhua` 数据源标注「个人使用；上架需自行确认授权」，
> CC-CEDICT 与 `ggml-small` 权重在工程内尚未补书面许可标注。
> 当前为 ad-hoc 分发版本，**正式上架前需完成许可审查**（工程内 QA 用例 `TC-SEC-010` 仍为 Not Started）。

## 已知限制

- **本地神经 TTS 尚未接入**：当前发音用 macOS 系统音色（本机实测 59 个中英音色 quality 均为 compact，听感偏机械）；
  sherpa-onnx 方案已完成选型与离屏实测，未随本版本发布。
- **签名与公证**：当前为 ad-hoc，未做 Developer ID 签名与公证；上架决策（1.4 GB 模型是否继续随包）仍未定。
- **QA 发版门禁未通过**：测试执行率 42.5%、代码覆盖率未插桩，工程内部标记为「当前不可发版」。
  本仓库提供的为可运行构建产物，非正式上架版本。
- **词形变化与例句数据缺口**：ECDICT 29 分片只有 8 个字段、无 `exchange`，部分词条缺词形变化；
  `chinese-xinhua` 词语表偏古汉语，现代多字词需走「逐字释义」。
- **麦克风实时输入与热键链路尚未真机实测**；权限清单里 Pictures / Music / Movies 读写对词典并非必需，后续会收敛。

## 反馈

问题与建议欢迎开 [Issue](https://github.com/sunyazhou13/MTAIDict-Releases/issues)。

---

© 2026 sunyazhou.com
