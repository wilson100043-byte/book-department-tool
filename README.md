# Wiley 已購買與系所比對工具

這是純前端 GitHub Pages 小工具。使用者不需要安裝 Python、Docker、App，也不需要 API Key。

## 使用方式

1. 開啟 `index.html` 或 GitHub Pages 網址。
2. 上傳 Wiley 全庫 Excel。
3. 選擇待比對工作表，預設 `All Live`。
4. 如果已購買資料在同一個 Excel，選擇已購買工作表，預設 `TAEBDC複本202512`。
5. 如果已購買資料是另一個 Excel，可在「已購買 Excel（選填）」另外上傳。
6. 輸入學校名稱。
7. 貼上該校系所或學院描述。
8. 按「複製 AI 產生系所關鍵字 Prompt」。
9. 把 Prompt 貼到 ChatGPT、Gemini 或其他 AI。
10. 把 AI 回傳的 CSV 整段貼回「AI 回傳結果」。
11. 按「開始比對並產生 Excel」。
12. 下載結果 Excel。

## AI 回傳格式

AI 必須只回傳 CSV，不要 Markdown、不要說明文字。

```csv
學校名稱,系所名稱,關鍵字
淡江大學,財務金融學系,財務金融;金融;投資;corporate finance;investment;portfolio;banking
淡江大學,會計學系,會計;審計;稅務;accounting;auditing;taxation
淡江大學,電機工程學系,電機;電子;半導體;electrical engineering;electronics;semiconductor
```

## 輸出

輸出一個工作表：`全部比對結果`。

原始欄位全部保留，只新增：

- `是否已購買`
- `系所`

輸出 Excel 會設定篩選箭頭，可在 Excel 中篩選 `是否已購買 = 未購買`。

## 單一 Prompt 範本

工具畫面會依照你輸入的學校名稱與系所描述自動產生完整 Prompt。下面是固定格式：

```text
你是一位圖書館電子書選書與學科分類助理。請把我提供的學校系所描述，整理成可貼回書單比對工具的 CSV，並同時為每個系所產生適合 Wiley 英文書目比對的關鍵字。

重要：請只輸出 CSV，不要 Markdown，不要程式碼區塊，不要任何說明文字。

CSV 輸出規則：
1. 第一列固定為：學校名稱,系所名稱,關鍵字
2. 每一列只放一個系所。
3. 學校名稱必須完全使用我指定的學校名稱。
4. 系所名稱請使用完整正式名稱；如果我只給簡稱，請合理補成正式系所名稱。
5. 關鍵字用半形分號 ; 分隔，不要用逗號分隔關鍵字。
6. 每個系所至少產生 12 個關鍵字。
7. 關鍵字要包含中文與英文。
8. 英文關鍵字要包含學科正式名稱、常見書名詞、研究主題詞、Wiley 書目標題或主題分類可能出現的詞。
9. 避免太廣泛的單詞，例如 research、introduction、theory、practice、management 不要單獨列入，除非和具體領域組成片語。
10. 不要加入其他學校的系所。
11. 不要合併不同系所。
12. 如果一個學院描述是跨領域或新興學院，請依描述建立合理的系所或領域名稱，並產生對應關鍵字。
```

## 部署到 GitHub Pages

把這些檔案放到 GitHub repository：

```text
index.html
README.md
使用說明.txt
系所關鍵字範本.csv
AI產生系所關鍵字Prompt.txt
```

到 GitHub repository 的 `Settings > Pages` 啟用 GitHub Pages，即可用網址開啟。

`index.html` 會從 CDN 載入 SheetJS Excel 元件，所以 repository 不需要放 `xlsx.full.min.js`。

## 隱私

所有 Excel 和 CSV 都在使用者瀏覽器本機處理，不會上傳到伺服器。工具本身不呼叫 AI API。
