# DEVLOG（開發紀錄，時間軸）

遊戲：單檔 HTML 速算小遊戲（K1 程度）。玩：https://demon-kids.vercel.app
另：https://arcrek-coder.github.io/demon/ ；repo：`Arcrek-coder/demon`（原名已改為 `demon`）。

## 決策（Why）
- **短 URL**：Vercel 預設域名無法移除帳號 slug，改用專案域名 `demon-kids.vercel.app`；`vercel.json` 上 no-cache 表頭，避免舊 HTML 被快取。
- **匿名**：不幫角色取名，一律用「女孩 / 魔王」等描述性稱呼。
- **星星只管題目內容**：`STAR[star] = {ops, max, cap, neg}`；每題秒數、擊退/前進秒數、打退所需題數是獨立設定（⚙️），與星級脫鉤 → `SK = ['ops','max','cap','neg']`。
- **紀錄**：每打退一次存 1 筆（時間、星級、⚙️ 值、答對/答錯、該局用時），`localStorage` key `math-demon-chase-records`，只留最近 60 筆；⚙️ 與星級預設不同時標示「自訂」。
- **動畫順序**：必須是 蓄力 → 射出 → **擊中** → 才把魔王推回（先前寫在 submit 當下，使用者反映「還沒打到就被推回」）。所以 push 存在 `magic.push`，在 `step()` 的 impact 分支套用；`dist` 對 `distTarget` 做 lerp，看起來是平滑滑行。

## 時間軸（commit）
2026-11-08
- `1f82829` 初版遊戲；`34bde69` Pages workflow；`e97bb63` 答對跳一下。
- `5a0706f` `vercel.json`；`3f8a75e` 測試 git 觸發部署；`e1dda40`/`26cf846`/`2d8224b`/`5ef64ef`/`7ef5962` 域名與自動部署驗證（測試檔已移除）。
- `2921788` repo/專案改名 `demon`；`245b37e` README 改匿名。
- `a212166` 星星 0–10 預設 + 獲勝紀錄；`68d48b5`/`6a1dd58` 星級選單移進 ⚙️ modal，主畫面保留狀態列；`1fbcdd7` 星星與秒數脫鉤。
- `037cde7` 魔法動畫：蓄力光球、法杖、魔法彈+trail、擊中環/閃光/震動/flinch、平滑推退。
- `04ee129` 修正時序（擊中後才推退）＋法杖（蓄力上抬、射出前挥）、砲口火花、女孩光環/舉起手臂、trail 與impact 環加大。
- `228e67a` 音響對應動畫：submit 播蓄力音、0.3s 後射出音、impact 播重擊音。

## 未做/可继续
- 手機瀏覽器實測（touch、音訊需要使用者互動才解锁 AudioContext）。
- 紀錄匯出/匯入（目前只在 localStorage，綁 origin，换域名不會跟著）。
- 動畫截圖驗證：本環境無法跑瀏覽器/Playwright。
