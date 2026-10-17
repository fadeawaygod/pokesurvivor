# 上線 pokesurvivor.com 自訂網域

網域已於 **Cloudflare** 購買。前端程式碼與資源均已就緒，只需完成下列標記為 **【你手動操作】** 的帳號設定步驟與最終上線流程。

當前狀態（2026-10-10）：

- [x] 已於 Cloudflare 購買網域（Cloudflare DNS 伺服器運作中）。
- [x] 已將 `pokesurvivor.com` 與 `www.pokesurvivor.com` 加入 Firebase 授權網域清單（Google 登入功能可直接正常運作）。
- [x] 西班牙文與英文頁面、sitemap.xml、robots.txt、404 頁面與社交分享圖（OG Image）均已生成並完成測試（Lighthouse 檢測：SEO 100 分、無障礙 100 分）。
- [x] 上線發布腳本測試完畢：`tools/activar_dominio.sh`。
- [x] 步驟 1 · 設定 Cloudflare DNS（**【你手動操作】**）
- [x] 步驟 2 · 在 GitHub 驗證網域擁有權（**【你手動操作】**，強烈建議）
- [x] 步驟 3 · 最終發布步驟（**自動化/工程處理**）：已於 2026-10-10 完成，具備 HTTPS
- [ ] 步驟 4 · 提交至 Google Search Console 與 Bing（**【你手動操作】**）

---

## 步驟 1 · 設定 Cloudflare DNS（【你手動操作】，約 5 分鐘）

1. 登入 <https://dash.cloudflare.com> → 點選 **pokesurvivor.com** → 進入左側選單 **DNS → Records**。
2. 點擊 **Add record** 新增以下 9 筆記錄。請注意：每一筆的 **Proxy status（代理狀態）** 必須切換為 **DNS only**（呈現**灰色雲朵**，切勿使用橘色雲朵）。

   | 類型 (Type) | 名稱 (Name) | IPv4 / IPv6 / 目標 (Target) |
   |---|---|---|
   | A | `@` | `185.199.108.153` |
   | A | `@` | `185.199.109.153` |
   | A | `@` | `185.199.110.153` |
   | A | `@` | `185.199.111.153` |
   | AAAA | `@` | `2606:50c0:8000::153` |
   | AAAA | `@` | `2606:50c0:8001::153` |
   | AAAA | `@` | `2606:50c0:8002::153` |
   | AAAA | `@` | `2606:50c0:8003::153` |
   | CNAME | `www` | `jpter78.github.io` |

   以上 IP 位址為 GitHub Pages 官方託管伺服器。

> **為什麼必須使用灰色雲朵（DNS Only）：**
> 若啟用橘色代理雲朵，Cloudflare 流量會介入中轉，導致 GitHub Pages 無法順利簽發自動 HTTPS SSL 憑證；且若 Cloudflare 的 SSL 設定為 "Flexible"，會引發無限重新導向迴圈。灰色雲朵能保證一次設定成功。

可選免費功能（同一控制面板內）：
- **DNSSEC**（選單 DNS → Settings → Enable DNSSEC）：保護網域免受 DNS 偽造劫持。
- **Email Routing**：免費設定 `contacto@pokesurvivor.com` 信箱轉發至個人 Gmail，提供官方品牌信箱形象。

完成後請告知 **「DNS 已設定完成」**，系統將檢驗全球 DNS 解析狀態（通常需 5 分鐘至 1 小時生效）。

## 步驟 2 · 在 GitHub 驗證網域擁有權（【你手動操作】，建議操作，約 3 分鐘）

保護專屬網域：防止其他未授權的 GitHub 帳號在 Pages 中綁定 `pokesurvivor.com`。

1. 登入 <https://github.com/settings/pages>（使用 JPTER78 帳號）。
2. 點擊 **Add a domain** → 輸入 `pokesurvivor.com` → 點擊 **Add domain**。
3. GitHub 會顯示一筆 **TXT** 驗證記錄：包含格式如 `_github-pages-challenge-JPTER78` 的名稱與一串雜湊代碼。
4. 返回 Cloudflare → DNS → Records → 點擊 **Add record**：類型選擇 **TXT**，名稱填入 GitHub 給予的名稱（不帶結尾的 `.pokesurvivor.com`），內容貼上雜湊代碼。
5. 返回 GitHub 點擊 **Verify**。若尚未通過，請稍候數分鐘後重試。

## 步驟 3 · 最終發布步驟（自動化/工程處理）

當 DNS 解析生效後：

1. 執行 `bash tools/activar_dominio.sh`：構建 SEO 靜態頁面、生成 `CNAME` 檔案並檢查確保未夾帶敏感資料。
2. 推送至 GitHub，並於 GitHub Pages 設定自訂網域 `pokesurvivor.com`。
3. 等待 GitHub 頒發 SSL 憑證後勾選 **Enforce HTTPS**（強制 HTTPS）。
4. 在實體瀏覽器中全面驗證：主要網頁載入、`www.` 自動跳轉、舊版 `github.io` 跳轉至新網域、`/en/` 英文版介面、一般登入與 Google 登入、排行榜與多人合作功能。
5. 將 GitHub 儲存庫的 Website 欄位更新為 `https://pokesurvivor.com`。

> [!CAUTION]
> **注意訪客模式存檔：**
> 訪客存檔儲存在瀏覽器本機的 `localStorage` 中，並嚴格綁定網域名稱。原本在 `jpter78.github.io` 以訪客模式遊玩的玩家，切換至 `pokesurvivor.com` 後將無法直接讀取原訪客存檔（已註冊帳號的玩家不受影響，資料皆儲存在雲端）。如有玩家詢問，請建議其在網域切換前先註冊綁定帳號。

## 步驟 4 · 提交 Google Search Console 與 Bing（【你手動操作】）

1. 前往 <https://search.google.com/search-console> → **新增資源** → 選擇 **網域 (Domain)** → 輸入 `pokesurvivor.com`。
2. 由於網域託管於 Cloudflare，Google 支援**一鍵自動透過 Cloudflare 驗證**：點擊確認授權即可完成。
3. 進入 **Sitemaps** 選單 → 輸入 `sitemap.xml` → 點擊 **提交**。
4. 進入 **網址審查** 選單 → 輸入 `https://pokesurvivor.com/` → 點擊 **要求建立索引**。建議同步提交 `https://pokesurvivor.com/en/`、`https://pokesurvivor.com/guia/` 與 `https://pokesurvivor.com/pokedex/`。
5. Bing 搜尋引擎（涵蓋 DuckDuckGo 與 Ecosia）：前往 <https://www.bing.com/webmasters> → 選擇 **從 Google Search Console 匯入**。

提交後通常需要數天至兩週開始於搜尋引擎展現效果。

## 後續推廣

- 更新 YouTube 宣傳影片說明欄、Ko-fi 頁面與各社群平台的遊戲連結。
- 更多 SEO 推廣策略可參閱 [SEO.md](SEO.md) 第 3 節（外部連結建立、itch.io、Reddit、Discord 社群推廣等）。
