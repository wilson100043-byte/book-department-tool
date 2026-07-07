# 已購買與系所比對工具

這是純前端 GitHub Pages 小工具。使用者不需要安裝 Python、Docker、App，也不需要 API Key。

## 使用方式

1. 開啟 `index.html` 或 GitHub Pages 網址。
2. 上傳 Wiley 全庫 Excel。
3. 選擇待比對工作表，預設 `All Live`。
4. 選擇已購買工作表，預設 `TAEBDC複本202512`。
5. 輸入學校名稱。
6. 上傳或貼上系所關鍵字 CSV，可先用 `系所關鍵字範本.csv` 測試。
7. 按「開始比對並產生 Excel」。
8. 下載結果 Excel。

## 輸出

輸出一個工作表：`全部比對結果`。

原始欄位全部保留，只新增：

- `是否已購買`
- `系所`

輸出 Excel 會設定篩選箭頭，可在 Excel 中篩選 `是否已購買 = 未購買`。

## 系所關鍵字 CSV 格式

```csv
學校名稱,系所名稱,關鍵字
淡江大學,財務金融學系,財務金融;金融;投資;corporate finance;investment
淡江大學,會計學系,會計;審計;稅務;accounting;auditing
淡江大學,電機工程學系,電機;電子;半導體;electrical engineering;electronics
```

可直接複製或修改資料夾中的 `系所關鍵字範本.csv`。

## 部署到 GitHub Pages

把這些檔案放到 GitHub repository：

```text
index.html
README.md
使用說明.txt
系所關鍵字範本.csv
```

到 GitHub repository 的 `Settings > Pages` 啟用 GitHub Pages，即可用網址開啟。

`index.html` 會從 CDN 載入 SheetJS Excel 元件，所以 repository 不需要放 `xlsx.full.min.js`。

## 隱私

所有 Excel 和 CSV 都在使用者瀏覽器本機處理，不會上傳到伺服器。
