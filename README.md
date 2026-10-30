# humanizer-zh-tw（Claude Code plugin）

由 [Rufus Hsu](https://github.com/rufushsu9987) 維護。改寫自 [kevintsai1202/Humanizer-zh-TW](https://github.com/kevintsai1202/Humanizer-zh-TW)，把其中兩個 skill 包裝成 Claude Code plugin；非原作者發布的官方版本。

| 元件 | 名稱 | 用途 |
| --- | --- | --- |
| Skill | `humanizer-zh-tw:humanizer-zh-tw` | 潤飾中文文字，刪去空話、重複與公式化表達，把中國用語換成台灣用語，保留事實、確定程度和作者聲音 |
| Skill | `humanizer-zh-tw:text-watermark-cleaner-zh-tw` | 檢查／清理不可見 Unicode、zero-width、tag characters 與文字型 provenance |
| Command | `/humanizer-zh-tw:humanize` | 直接呼叫潤飾 skill，可用 `--sample` 指定風格樣本 |
| Command | `/humanizer-zh-tw:watermark` | 直接呼叫浮水印清理 skill（預設 `inspect`，只檢查不修改） |

Skill 也會依描述自動觸發，例如「幫我把這段去 AI 味」「改成台灣用語」。

## 讓改寫更像你寫的

給它一兩篇你自己寫的舊文章當樣本，改寫會模仿你的句長、用詞、語氣和標點習慣（只借表達方式，不會把樣本內容搬進來）：

```text
/humanizer-zh-tw:humanize draft.md --sample my-old-post.md
```

想每次都用同一份樣本，就在專案根目錄放一個 `.humanizer-voice.md`，沒指定 `--sample` 時會自動使用。樣本越長效果越好，建議至少 300 字。

## 目錄結構

```text
humanizer-zh-tw/
├── .claude-plugin/
│   ├── plugin.json          # plugin 清單
│   └── marketplace.json     # 讓本 repo 本身可當 marketplace 使用
├── commands/
│   ├── humanize.md
│   └── watermark.md
├── skills/
│   ├── humanizer-zh-tw/
│   │   ├── SKILL.md         # 約束、流程與 31 個檢查點索引
│   │   └── references/      # 檢查點完整範例、台灣用語對照、評分表
│   └── text-watermark-cleaner-zh-tw/
│       ├── SKILL.md
│       ├── references/
│       └── scripts/         # Python deterministic scripts + PowerShell 入口
└── tests/                   # 上游的案例與結構檢查腳本
```

## 安裝

### 本機測試

```bash
claude --plugin-dir ./humanizer-zh-tw
```

### 從本機 marketplace 安裝

在 Claude Code 中：

```text
/plugin marketplace add ./humanizer-zh-tw
/plugin install humanizer-zh-tw@humanizer-zh-tw
```

### 從 GitHub 安裝

在 Claude Code 對話中：

```text
/plugin marketplace add rufushsu9987/humanizer-zh-tw-plugin
/plugin install humanizer-zh-tw@humanizer-zh-tw
```

或在 shell 中：

```bash
claude plugin marketplace add rufushsu9987/humanizer-zh-tw-plugin
claude plugin install humanizer-zh-tw@humanizer-zh-tw
```

## 需求

`text-watermark-cleaner-zh-tw` 的腳本需要 Python 3。Windows 使用 `scripts/run-text-watermark.ps1`（透過 `py` launcher）；macOS／Linux 直接呼叫 `python3 -X utf8 scripts/inspect_text.py`。

## 與原 repo 的差異

- 改為 plugin 目錄結構（`skills/`、`commands/`、`.claude-plugin/`）。
- skill 互相引用改為 plugin 命名空間（`humanizer-zh-tw:...`），取代 Codex 的 `$skill` 寫法。
- 浮水印 skill 的腳本路徑改為相對於 skill 目錄，並補上 macOS／Linux 的呼叫方式。
- 移除 Codex 專用的 `agents/openai.yaml`。
- 新增台灣用語對照（`references/taiwan-wording.md`），潤飾時會把「視頻、軟件、默認」這類中國用語換成台灣說法。
- 新增作者風格樣本（`--sample`、`.humanizer-voice.md`）。
- 31 個檢查點的完整範例移到 `references/patterns.md`，SKILL.md 只留索引，減少每次載入的內容。
- 潤飾完成後不再每次主動詢問是否檢查浮水印。

## 授權與來源

MIT（見 [LICENSE](LICENSE)）。規則源自 [op7418/Humanizer-zh](https://github.com/op7418/Humanizer-zh) 與 [blader/humanizer](https://github.com/blader/humanizer)；浮水印腳本擷取自 [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)（MIT，授權見 `skills/text-watermark-cleaner-zh-tw/scripts/LICENSE-watermarks-remover.txt`）。更新紀錄見 [CHANGELOG.md](CHANGELOG.md)。
