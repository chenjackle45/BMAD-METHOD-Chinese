# BMad Hacker Daily Digest LLM 提示語句（Prompts）

本文件定義與設定之 Ollama LLM 互動時所使用的標準提示語句。統一集中管理這些 prompts 可確保輸出一致性並便於實驗與調整。

## 提示設計理念

這些提示語句的目標是引導 LLM（如 Llama 3 或其他類似模型）生成簡潔、具資訊量的摘要，聚焦於 BMad Hacker Daily Digest 的核心目標：快速掌握一篇文章或 HN 討論的精髓。

## 核心提示語句

### 1. 文章摘要提示

- **用途：** 摘要從網頁擷取的文章，提煉出主要觀點、論述與結論。
- **變數名稱（概念性）：** `ARTICLE_SUMMARY_PROMPT`
- **提示語句內容：**

```text
You are an expert analyst summarizing technical articles and web content. Please provide a concise summary of the following article text, focusing on the key points, core arguments, findings, and main conclusions. The summary should be objective and easy to understand.

Article Text:
---
{Article Text}
---

Concise Summary:
```

### 2. HN 討論摘要提示

- **用途：** 摘要 Hacker News 某則貼文下的留言，提取主要主題、觀點對立、重要洞見與整體情緒傾向。
- **變數名稱（概念性）：** `DISCUSSION_SUMMARY_PROMPT`
- **提示語句內容：**

```text
You are an expert discussion analyst skilled at synthesizing Hacker News comment threads. Please provide a concise summary of the main themes, diverse viewpoints (including agreements and disagreements), key insights, and overall sentiment expressed in the following Hacker News comments. Focus on the collective intelligence and most salient points from the discussion.

Hacker News Comments:
---
{Comment Texts}
---

Concise Summary of Discussion:
```

## 實作說明

- **占位符：** `{Article Text}` 與 `{Comment Texts}` 表示實際插入的文章或留言內容，由應用程式（如 `src/core/pipeline.ts` 或 `src/clients/ollamaClient.ts`）在呼叫 API 時動態注入。
- **載入方式：** MVP 階段建議將這些提示語句作為常數寫在程式碼中（如 `src/utils/prompts.ts` 或直接於 `ollamaClient` 中引用），並以本文件作為標準依據。未來版本可考慮於執行時動態讀取本文件。
- **持續優化：** 這些 prompts 僅為初始版本。預期在實際使用特定 `OLLAMA_MODEL` 模型後，根據摘要品質持續優化。

## 變更紀錄

| 變更內容 | 日期       | 版本 | 說明             | 作者        |
| -------- | ---------- | ---- | ---------------- | ----------- |
| 初始草稿 | 2025-05-04 | 0.1  | 初版提示語句定義 | 3-Architect |
