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
- 訓練台的「劇本」分頁是劇本庫，格式是「指引 → 鉤子 → 逗」。內建劇本放在 `trainer/data.js` 的 `ROUTINES`：
  `{id, title, stage, when, lead:{line, why}, hook:{line, why}, tease:{rule, why}, replies:[{step:"lead"|"hook", her, me}], guard}`。
  使用者自己新增的劇本存在 db。升溫類的劇本一定要寫 `when`（使用時機）和 `guard`（她不舒服時怎麼收回）。
- `prompts/navigator.md`：在 Claude App 專案裡用的導航指令。
- `bar/index.html`：酒吧口袋教練，現場搭訕用。只有深色主題、大字提詞卡，五個分頁：上場、口袋、話題、教練、紀錄。
  發布：Artifact publish `file_path: bar/index.html`, `files: {"data.js": "bar/data.js"}`, capabilities `{db:{}, user:{}, sample:{}}`，
  網址 https://claude.ai/artifact/VwH3GZoLrsoWpJoAiNo8nx。
- `bar/data.js`：現場內容（`STATES` 狀況台詞、`SCENES` 場景開場、`THREADS` 接話公式、`SIGNALS`、`TOPICS` 常青＋季節、`LADDER` 任務階梯、`DRILLS` 酒吧練習題）。
  話題只放內建的常青和季節題，不做時事。DRILLS 格式跟訓練台一樣，階段是 B0–B4。
  `ROUTINES` 是「升溫」狀況用的當面版劇本（唯讀，格式同訓練台）。兩個 Artifact 的 db 是分開的，
  在訓練台新增的劇本不會出現在這裡，要放進口袋教練就得改這個檔案。

## 資料
- 訓練台：答題紀錄、筆記、金句、AI 出的題目存在 db 的 `data/users/<uid>`（docs：`progress`、`n_*`、`l_*`、`g_*`、`r_*` 我的劇本），瀏覽器 localStorage 是備份。
- 口袋教練：`progress`（練習題）、`night_<YYYY-MM-DD>`（每晚的任務和開場結果；凌晨 6 點前算前一晚）、`n_*`（復盤與筆記）。
