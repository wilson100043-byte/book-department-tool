# 書單已購買與系所比對工具

這是純前端 GitHub Pages 小工具。使用者不需要安裝 Python、Docker、App，也不需要 API Key。

## 使用方式

工具分成兩個區塊，可以依需求擇一使用。

### 區塊一：書單已購買與系所比對

1. 開啟 `index.html` 或 GitHub Pages 網址。
2. 上傳出版社提供的待比對書單 Excel。
3. 選擇待比對工作表。如果檔案內有 `All Live`，工具會優先選用。
4. 如果已購買資料在同一個 Excel，選擇已購買工作表。
5. 如果已購買資料是另一個 Excel，可在「已購買 Excel（選填）」另外上傳。
6. 輸入學校名稱。
7. 貼上該校系所或學院描述。
8. 按「複製 AI 產生系所關鍵字 Prompt」。
9. 把 Prompt 貼到 ChatGPT、Gemini 或其他 AI。
10. 把 AI 回傳的 CSV 整段貼回「AI 回傳結果」。
11. 按「開始比對並產生 Excel」。
12. 先下載比對結果 Excel。
13. 如果要上傳到系統，再到最後一步填寫學校代碼、主題代碼、SDGs。
14. 按「產生系統上傳格式」並下載系統上傳格式 Excel。

這個區塊產生的系統上傳格式只會包含 `未購買` 書目。

### 區塊二：直接轉系統上傳格式

如果你已經有整理好的 Excel，不需要做已購買比對，可以直接使用下方「直接轉系統上傳格式」區塊。

1. 上傳來源 Excel。
2. 選擇來源工作表。
3. 填寫學校代碼、主題代碼、SDGs。
4. 按「直接產生系統上傳格式」。
5. 下載 `Tablib Dataset` 格式的 Excel。

這個區塊會把來源工作表所有資料列都轉出，不會篩選 `未購買`。

## AI 回傳格式

AI 必須只回傳 CSV，不要 Markdown、不要說明文字。

```csv
學校名稱,系所名稱,關鍵字
淡江大學,財務金融學系,財務金融;金融;投資;corporate finance;investment;portfolio;banking
淡江大學,會計學系,會計;審計;稅務;accounting;auditing;taxation
淡江大學,電機工程學系,電機;電子;半導體;electrical engineering;electronics;semiconductor
```

## 輸出

工具可輸出兩類 Excel。

### 比對結果 Excel

工作表：`全部比對結果`

原始欄位全部保留，只新增：

- `是否已購買`
- `系所`

輸出 Excel 會設定篩選箭頭，可在 Excel 中篩選 `是否已購買 = 未購買`。

### 系統上傳格式 Excel

工作表：`Tablib Dataset`

可由兩種方式產生：

- 在比對結果後產生：只輸出 `未購買` 書目。
- 直接上傳自己的檔案轉出：輸出來源工作表全部資料列。

欄位格式依舊工具的系統上傳格式：

```text
ID
學校代碼
主題代碼(全字母大寫)
題名(可大寫可小寫)
作者(可大寫可小寫)
出版者(可大寫可小寫)
出版年
ISBN
網址
SDGs(數字前後一定要有逗號)
主題(英文大小寫皆可，出版社主題若有逗號，請改為 &
若有多主題以逗號區隔。)
```

其中 `學校代碼`、`主題代碼`、`SDGs` 由畫面欄位帶入。`主題代碼` 會轉成大寫，`SDGs` 格式需像 `,9,` 或 `,9,12,`。

直接轉檔時，工具會優先辨識這些常見欄位名稱：

- 題名：`Title`、`題名`、`書名`
- 作者：`First Author Full Name`、`作者`、`Author`
- 出版者：`Imprint`、`出版者`、`Publisher`、`出版社`
- 出版年：`Copyright Year`、`出版年`、`Year`
- ISBN：`O-Book ISBN13`、`E-Book ISBN13`、`Print ISBN13`、`O-ISBN_比對`、`ISBN`、`ISBN13`、`ISBN-13`、`電子書ISBN`、`紙本ISBN`、`國際標準書號`
- 網址：`網址`、`URL`、出版社提供的書目網址欄位
- 主題：`Specialized Subject Area`、`主題`、`Main Subject Category`

## 單一 Prompt 範本

工具畫面會依照你輸入的學校名稱與系所描述自動產生完整 Prompt。下面是固定格式：

```text
你是一位圖書館電子書選書與學科分類助理。請把我提供的學校系所描述，整理成可貼回書單比對工具的 CSV，並同時為每個系所產生適合出版社書目比對的關鍵字。

重要：請只輸出 CSV，不要 Markdown，不要程式碼區塊，不要任何說明文字。

CSV 輸出規則：
1. 第一列固定為：學校名稱,系所名稱,關鍵字
2. 每一列只放一個系所。
3. 學校名稱必須完全使用我指定的學校名稱。
4. 系所名稱請使用完整正式名稱；如果我只給簡稱，請合理補成正式系所名稱。
5. 關鍵字用半形分號 ; 分隔，不要用逗號分隔關鍵字。
6. 每個系所至少產生 12 個關鍵字。
7. 關鍵字要包含中文與英文。
8. 英文關鍵字要包含學科正式名稱、常見書名詞、研究主題詞、出版社書目標題、摘要或主題分類可能出現的詞。
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
