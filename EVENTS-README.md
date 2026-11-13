# 護理學系活動日程｜Google Sheets → GAS → GitHub Pages → RPage

本儲存庫專門管理學系重要活動日程，與教師資料專案分開。

## 檔案
- `index.html`：網站首頁，導向活動日程。
- `events.html`：活動表格（搜尋、對象/類別/年份篩選、響應式）。
- `data/events.json`：目前已發布的活動快照，前端讀取此檔。
- `gas/Code.gs`：貼到該 Google Sheet 的「擴充功能 → Apps Script」。
- `rpage-events-embed.html`：貼到 RPage HTML 區塊的嵌入範例。

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
9. 在 RPage HTML 區塊貼入 `rpage-events-embed.html` 的內容，或先貼單純的 iframe 測試環境限制。

## 維護規則
- 一列一筆；不需維護「顯示順序」和「活動ID」欄位。
- 日期為 `YYYY-MM-DD`，活動時間可填 `1600-1800` 或 `16:00–18:00`。
- 「是否發布」填 **是** 才會輸出，填 **否** 會保留於試算表但不發布。
- 網頁**完全依照 Google Sheet 工作表的列順序**呈現，不按日期重新排序。調整順序請移動整列，然後重新發布。
- JSON 中的 `id` 由 GAS 根據目前的實際列號自動產生，例如第 2 列是 `row-0002`。移動、插入或刪除列時，ID 會跟著改變；請勿將它視為永久不變的活動識別碼。
- GAS 只會覆蓋 `data/events.json`，不會修改其他網頁。
- 網站已存在一份初始 JSON；之後每次要反映新資料，都要按 **發布正式 JSON**。
- 如果 RPage 不允許執行 script，高度自動調整可能失效；可只用 iframe 固定高度。
