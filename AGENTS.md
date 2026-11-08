# AGENTS.md（給接手者）

單檔 HTML 遊戲，無框架、無構建步驟。先讀 `README.md`（規格）與 `DEVLOG.md`（決策/時間軸）。

## 結構
- `index.html`：唯一執行檔（HTML + CSS + JS，JS 在單一個 `<script>`）。
- `girl.svg`、`yellow_character.svg`、`rainbow.svg`：靜態 SVG，透過 `art(IMG.x, AR.x, cx, cy, size, alpha)` 繪到 canvas；缺檔時退回 `drawKnight()` / `drawDemon()`。
- `vercel.json`：no-cache 表頭。`.github/workflows/pages.yml`：GitHub Pages 部署。

## 不變量（改動前確認）
- 星級**只**決定題目內容：`STAR[k] = {ops, max, cap, neg}`；`SK = ['ops','max','cap','neg']`。時間/題數/紀錄與星級無關。
- 0–4★ 只有加法；5★ 起有減法（大數放前，答案不為負）；9★ 起允許負答案。
- 推進/推退：`distTarget` 每幀減 `dt/C.time`；`dist` 向其 lerp。答對的 push 存在 `S.magic.push`，**只在 `step()` 的 impact 分支**（`m.t >= 1`）套用 → 先打到才推回。被抓的判斷用 `S.dist <= 0`，結算用 `S.dist >= 1`。
- 動畫時間：`charge = 0.3`（蓄力）+ `flight = 0.45` → `dur = 0.75`；聲音用 `setTimeout` 對應同一時間軸。
- `localStorage`：`math-demon-chase-settings`（⚙️）、`math-demon-chase-records`（最近 60 筆）。
- `beep(freqs, dur, type, gain)` 需要使用者互動後 AudioContext 才會解鎖（`clickSfx()`）。

## 驗證 / 部署
1. 語法：抽出 `<script>` 內容 → `node --check`（用 `encoding='utf-8'` 寫檔）。
2. `git push origin main` → Vercel 30–60 秒生效；確認：`curl -s https://demon-kids.vercel.app/ | grep -c '<某個新字串>'`。
3. 本機不能跑瀏覽器/Playwright，無法截圖；改動畫只能靠讀程式邏輯 + 使用者回報。

## 踩過的坑
- git 可能把工作副本轉成 **CRLF**，精確字串匹配/編輯會失敗 → 先確認 `d.count(b'\r\n')`，必要時轉 LF。
- Windows 預設編碼是 cp950，寫檔要 `encoding='utf-8'`（含 `⚙️`、`★` 會炸）。
- `localStorage` 綁 origin：`demon-kids.vercel.app` 與 `arcrek-coder.github.io` 是兩份紀錄。
- 變數 `jump` 已改名 `jh`；`parts()` 不再依賴它。
