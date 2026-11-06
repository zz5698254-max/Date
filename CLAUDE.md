# Date — 交友聊天訓練工具

使用者都是在手機上用 Claude Code 做 vibe coding。回覆用台灣繁體中文。

## 原則
- 不用付費 API。AI 功能走 Artifact 的 `sample` capability（用使用者自己的 Claude 額度）。
- 做「副駕駛」不做「自動駕駛」：不做自動右滑、自動代聊。
- 推拉＝肯定＋調侃。不教打壓（negging）、製造競爭焦慮、情緒勒索。

## 結構
- `trainer/index.html`：調頻訓練台（UI＋邏輯），以 Artifact 發布，`data.js` 一起放在 `files`。
  發布：Artifact publish `file_path: trainer/index.html`, `files: {"data.js": "trainer/data.js"}`,
  網址 https://claude.ai/artifact/4mnw6R3jFnPfrLccVXQoMb（從其他對話更新時帶 `url`，先 read）。
- `trainer/data.js`：所有內容（階段、技巧、心法、誤區、情境題）。加題目只要改這裡。
  題目格式：`{id, stage, title, profile?, context, chat:[{who:"her"|"me"|"sys", text}], options:[{text, score 0-3, tags, why}] ×4, subtext, read, best:[], principle, drill}`。
  每題一個 3 分、一個 0 分；tags 必須是 `TAGS` 裡的 id。
- `prompts/navigator.md`：在 Claude App 專案裡用的導航指令。

## 資料
答題紀錄、筆記、金句、AI 出的題目存在 db 的 `data/users/<uid>`（docs：`progress`、`n_*`、`l_*`、`g_*`），瀏覽器 localStorage 是備份。
