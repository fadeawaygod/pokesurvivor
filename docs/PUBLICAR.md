# 免費發布遊戲至 GitHub Pages

本遊戲為純靜態網頁，可透過 GitHub Pages 免費託管並提供線上訪問（例如：`https://jpter78.github.io/pokesurvivor/`）。

## 1. 建立 GitHub 儲存庫（只需操作一次）

1. 使用 **JPTER78** 帳號登入 <https://github.com/new>。
2. 專案名稱填寫：`pokesurvivor` · 設為 **Public (公開)** · **不要勾選**新增 README、.gitignore 或授權條款（專案中已包含）。
3. 點擊「Create repository」建立儲存庫。

## 2. 推送專案程式碼

在專案本地根目錄下（本地 Git 儲存庫已初始化並已完成 initial commit）：

```bash
git remote add origin https://github.com/JPTER78/pokesurvivor.git
git push -u origin main
```

初次推送時會彈出視窗要求驗證 GitHub 帳號。
專案容量約 75 MB（主要為寶可夢精靈圖資），可能需要幾分鐘完成上傳。

## 3. 啟用 GitHub Pages

在 GitHub 儲存庫頁面中：
前往 **Settings → Pages** →
*Source* 選擇：**Deploy from a branch** → *Branch* 選擇：**main**，目錄選擇：**/ (root)** → 點擊 **Save**。

約一至兩分鐘後，即可在 <https://jpter78.github.io/pokesurvivor/> 訪問遊戲。

## 4. 串接 Firebase 雲端服務

若您已按照 [FIREBASE.md](FIREBASE.md) 完成設定，請記得將 `jpter78.github.io` 加入至 Firebase Authentication 的**已授權網域**清單（步驟 5），並將填妥的 `firebase-config.js` 推送至儲存庫：

```bash
git add js/core/firebase-config.js
git commit -m "Conectar con Firebase"
git push
```

## 5. 提交 Google 搜尋引擎收錄（SEO）

1. 前往 <https://search.google.com/search-console> → **新增資源** →
   *網址前置字元* 輸入 → `https://jpter78.github.io/pokesurvivor/`。
2. 驗證所有權：選擇 **HTML 標記** 驗證方式，取得 `<meta ...>` 標籤程式碼；將該行貼入 `index.html` 的 `<head>` 區塊中，執行 `git push`，然後在 Search Console 點擊「驗證」。
3. 前往 **網址審查** → 貼上遊戲網址 → 點擊 **要求建立索引**。

Google 通常需要數天至數週進行檢索與收錄，初期通常只有在精確搜尋遊戲名稱時才會呈現結果。

## 後續更新遊戲版本

每當您進行程式碼或內容修改時：

```bash
git add -A
git commit -m "本次更新說明的內容"
git push
```

GitHub Pages 會在一至兩分鐘內自動部署最新版本。

## 重要注意事項

- **絕對嚴禁任何營利行為**：不得置入廣告、不得設有付費解鎖內容或強制贊助門檻。此為精靈圖素材授權條款（CC BY-NC 4.0 非商業性使用）的強制要求，亦能最大程度降低任天堂（Nintendo）版權警告的法律風險。
- 若法定聯絡信箱收到版權方或權利人的下架通知，請務必第一時間積極配合處理：迅速移除相關爭議內容通常即可平息爭端。
