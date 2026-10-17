# SEO 與 pokesurvivor.com 網域營運指南

所有前端技術層面的 SEO 優化已在程式碼中配置完畢。剩下的是需要手動操作的部分（購買網域、DNS 配置以及向搜尋引擎登錄網站）。

## 技術層面已完成的配置

| 檔案 / 目錄 | 功能與配置說明 |
|---|---|
| `index.html` | 專為搜尋優化的標題與敘述（"免費線上寶可夢遊戲"、"類 Survivor.io 風格"）、標準網址 (Canonical URL)、社群分享卡片（Open Graph / Twitter Card）、`VideoGame` 結構化資料（免費、1-4人、支援瀏覽器/Android/iOS）以及載入時的友善 HTML 標題文字。 |
| `guia/` | 新手指南頁面：包含操作教學、遊戲特色、實用技巧與**常見問題 FAQ**（配置 `FAQPage` 結構化資料，有機會在 Google 搜尋結果中呈現摺疊問答富文本摘要）。 |
| `pokedex/` | 收錄 966 隻可遊玩寶可夢與其屬性、精靈圖：吸引搜尋「免費皮卡丘遊戲」等長尾關鍵字。 |
| `404.html` | 具備返回遊戲連結的自訂 404 頁面（GitHub Pages 自動採用）。 |
| `robots.txt` | 允許檢索除 `tools/` 與 `docs/` 以外的所有資源，並指向 Sitemap。 |
| `sitemap.xml` | 完整定義待檢索收錄的頁面清單。 |
| `assets/og-image.jpg` | 社群平台分享時的預覽大圖（1200×630）。 |
| `manifest.webmanifest`, 圖示 | PWA 漸進式應用安裝配置、瀏覽器與行動裝置桌面高畫質圖示。 |

相關靜態頁面可透過 `python tools/make_seo_pages.py` 自動重新生成（例如當寶可夢資料或特性更新時）。

> [!IMPORTANT]
> 目前標準網址（Canonical）皆已指向 `https://pokesurvivor.com/`。請確認自訂網域正式上線生效後再行全面發布，避免搜尋引擎檢索尚未解析的網域名稱。

## 1. 網域設定

啟用 pokesurvivor.com 的詳細步驟（Cloudflare DNS、GitHub Pages、Firebase 授權與上線清單）請參閱 [DOMINIO.md](DOMINIO.md)。

## 2. Google Search Console 提交（最關鍵）

1. 使用 Google 帳號登入 <https://search.google.com/search-console>。
2. **新增資源 → 網域** → 輸入 `pokesurvivor.com`。
3. 取得 Google 提供的 **TXT** 驗證記錄：在 Cloudflare DNS 新增（名稱填 `@`）並於 Search Console 點擊驗證。
4. 進入 **Sitemaps** → 提交 `sitemap.xml`。
5. 進入 **網址審查** → 輸入 `https://pokesurvivor.com/` → 點擊 **要求建立索引**。建議對 `/guia/` 與 `/pokedex/` 重複此操作。

Bing 搜尋引擎（涵蓋 DuckDuckGo 與 Ecosia）：登入 <https://www.bing.com/webmasters> → 選擇「從 Google Search Console 匯入」。

## 3. 站外推廣與反向連結（提升排名的核心）

搜尋引擎極度看重權威網站的反向連結與品牌關鍵字搜尋量：

- **YouTube 實機展示或宣傳片**：在說明欄第一行置入遊戲連結，影片標題可命名為「PokéSurvivor – 免費瀏覽器寶可夢倖存者遊戲」。
- **itch.io**：在 HTML5 分類建立免費遊戲展示頁，並附上正式遊戲官網連結。
- **Reddit**：於 r/PokemonROMhacks、r/incremental_games、r/WebGames、r/survivorslikes 等相關板塊分享（請遵守各板板規，多數社群非常歡迎無廣告的免費同人自製遊戲）。
- **Discord**：於寶可夢同人社群或類吸血鬼倖存者愛好者伺服器中分享交流。
- **短影音平台（TikTok / Shorts / Reels）**：剪輯 15 秒精華短片（例如遭遇野外異色寶可夢、傳說寶可夢裂縫挑戰、4人連線混戰等）。
- 邀請實況主或玩家體驗並分享官網連結。

## 4. 長期維護策略

- 定期於 Search Console 觀察帶來流量的**熱門搜尋字詞**，並於新手指南中擴充相應的解答。
- 推出重大版本更新時：在 `novedades/` 新增更新紀錄並加入 Sitemap。
- 頁面加載速度維持極致流暢（Lighthouse 效能與最佳實踐皆達標）。

## 法律版權注意事項

「寶可夢（Pokémon）」為任天堂與 The Pokémon Company 註冊商標。使用 PokéSurvivor 作為專案名稱與獨立網域仍有潛在收到版權聲明的可能性；維持非營利、無內購、無廣告以及完整的免責聲明有助於降低風險，但無法完全免除。若接獲官方版權通知，應積極配合處理。

## 開發與發布工作流

- **日常開發編輯 `dev.html`**（以及 `js/` 底下的獨立源碼模組），切勿直接修改 `index.html`。`dev.html` 分開載入每個腳本，便於開發與除錯；`index.html` 為正式發布版本，將所有邏輯打包至 `js/app.min.js` 中。
- 發布前必須依序執行：

```bash
node tools/build.js            # 將 dev.html 打包為 index.html + js/app.min.js
node tools/export_data.js      # 匯出遊戲資料供靜態頁面生成
python tools/make_seo_pages.py # 生成多語言頁面、en/index.html 與 sitemap
```

## 語系支援

- 遊戲支援英文與西班牙文，依據玩家瀏覽器語言自動呈現，亦可於「設定 → 畫面 → 語言」中切換。`/en/` 目錄直接開啟英文版。
- 英文文字資源：集中於 `js/data/lang-en.js`（鍵值為西班牙文原文）。若新增文字需同步補齊對應鍵值。
- 各靜態子頁面（指南、圖鑑、傳說寶可夢、異色寶可夢、屬性、招式、道具、成就與更新公告）皆具備雙語版本，並配置 `hreflang` 相互關聯。
