---
description: 檢查或清理文字中的不可見 Unicode、zero-width、tag characters 與文字型 provenance 標記
argument-hint: [inspect|layer-a|layer-b|full] <檔案路徑或文字>
---

使用 `humanizer-zh-tw:text-watermark-cleaner-zh-tw` skill 處理以下請求，完整遵循該 skill 的工作邊界、模式選擇與報告格式。

- 第一個參數若是 `inspect`、`layer-a`、`layer-b` 或 `full`，視為模式；未指定時預設 `inspect`，只檢查不修改。
- 內附腳本位於 `${CLAUDE_PLUGIN_ROOT}/skills/text-watermark-cleaner-zh-tw/scripts/`。清理時保留原檔，輸出 `*.cleaned.*` 副本。
- 單純要求文字更自然時，改用 `humanizer-zh-tw:humanizer-zh-tw`。

請求內容：

$ARGUMENTS
