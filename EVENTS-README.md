# 護理學系活動日程｜Google Sheets → GAS → GitHub Pages → RPage

本儲存庫專門管理學系重要活動日程，與教師資料專案分開。

## 檔案
- `index.html`：網站首頁，導向活動日程。
- `events.html`：活動表格（搜尋、對象與類別篩選、響應式方正介面）。
- `data/events.json`：目前已發布的活動快照，前端讀取此檔。
- `gas/Code.gs`：貼到該 Google Sheet 的「擴充功能 → Apps Script」。
- `rpage-events-embed.html`：包含 `<style>`、HTML、`<script>` 的雙語嵌入檔；無 iframe、無標題，可直接貼到 RPage HTML 區塊。

## 資料來源
Google Sheet：
https://docs.google.com/spreadsheets/d/1fzyEAfJw1BsyWRvVrF1VnCmQQPseMoqLOjY_3RvBplY/edit

已預先整理護理學系公開頁面列出的 15 筆 2026 年活動，初次上線請由管理者再核對。

## 首次啟用發布
1. 使用 `bmstweb806@gmail.com` 開啟上面的 Google Sheet。
2. 點 **擴充功能 → Apps Script**，將 `gas/Code.gs` 全部複製到預設 `Code.gs`，儲存。
3. GAS 專案設定將時區設為 **Asia/Taipei**。
4. GitHub 使用者 `bmstweb806` 建立 fine-grained personal access token：僅授權 `nsysu-calendar` repository，`Contents: Read and write`。
5. GAS **專案設定 → 指令碼屬性** 新增 `GITHUB_TOKEN` 並貼入 token。**絕不將 token 放進 Sheet 欄位、GitHub、HTML、RPage。**
6. 重新整理 Google Sheet，會出現 **活動日程發布** 選單。
7. 先選 **檢查資料**，再選 **發布正式 JSON**；第一次需同意 Apps Script 的 Google 權限。
8. GitHub Pages 網址： https://bmstweb806.github.io/nsysu-calendar/events.html
9. 在 RPage 的 HTML 原始碼區塊貼入 `rpage-events-embed.html` 完整內容（包含 `<style>`、HTML、`<script>`），不需要 iframe。

## 維護規則
- 一列一筆；不需維護「顯示順序」和「活動ID」欄位。
- 日期為 `YYYY-MM-DD`，活動時間可填 `1600-1800` 或 `16:00–18:00`。
- 「是否發布」填 **是** 才會輸出，填 **否** 會保留於試算表但不發布。
- 同一筆活動的 A:I 是既有中文資料與發布設定；J:N 是英文「對象／類別／活動項目／地點／備註」。日期、時間與連結共用。
- 英文內容為初步翻譯草稿，正式對外發布前請由系所確認。若英文欄位留空，英文網頁會以中文原文作為備援。
- 網頁**完全依照 Google Sheet 工作表的列順序**呈現，不按日期重新排序。調整順序請移動整列，然後重新發布。
- JSON 中的 `id` 由 GAS 根據目前的實際列號自動產生，例如第 2 列是 `row-0002`。移動、插入或刪除列時，ID 會跟著改變；請勿將它視為永久不變的活動識別碼。
- GAS 只會覆蓋 `data/events.json`，不會修改其他網頁。
- 網站已存在一份初始 JSON；之後每次要反映新資料，都要按 **發布正式 JSON**。
- RPage 需允許 `<style>`、`<script>` 與跨站 `fetch()`；若平台移除 script，資料便無法載入，需調整 RPage 編輯權限或嵌入方式。
- **雙語切換**：RPage 中文網址 `?Lang=zh-tw`、英文網址 `?Lang=en` 會決定初始語言；若網址沒有設定則參考頁面 HTML 的 `lang`，預設中文。訪客仍可使用方正的 **「中文｜EN」按鈕** 隨時切換，無下拉選單；切換會清除舊語言的搜尋與篩選條件。
- 介面不提供年份篩選，資料仍保留完整活動日期；「對象」使用**簡潔單行框線標籤**與年級代碼（如 `Y1｜一年級`、`ALL｜全系`），「類別」使用單行實心色塊及圖記（如 `◆ 活動`、`▤ 課程`、`◎ 輔導`）。
- 桌面版活動表格分配固定欄寬，避免標籤被擠成直排；在較窄畫面切換為直角卡片。英語版標籤也採用短名稱，但滑鼠移上去仍可看到完整英文內容。
- 所有控制元件和行動版活動卡片均使用方角（無圓角）。
- 對象下拉選項固定為 15 種：「本系全體學生」、「本系學士班／碩士班／博士班學生」、「本系大一～大四生」、「本系碩一～碩二生」、「本系博一～博四生」、「本系教師」。個別選項依序列出；前端標籤使用 `ALL / UG / MS / PHD / Y1–Y4 / M1–M2 / D1–D4 / FAC` 等代碼。
- 類別下拉選項固定為 5 種：「活動」、「課程」、「輔導」、「招生」、「其他」，並提供對應英文選項。新增招生標籤 `✦ 招生 / Admissions`。
- 更動分類時，請同步選擇 J 欄的對象英文與 K 欄的類別英文；GAS 會直接輸出這些雙語欄位。
- 獨立頁面也支援 `https://bmstweb806.github.io/nsysu-calendar/events.html?Lang=en`。
