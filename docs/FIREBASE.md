# 設定 Firebase（雲端資料庫與帳號系統）

完成設定後，遊戲存檔即可儲存至雲端，並支援跨裝置無縫接續遊玩。所有項目均在 **Firebase 免費 Spark 方案**內運行，無任何費用。整個設定流程約需 10 分鐘。

在未進行此設定前，遊戲依然可完全正常運行，預設將存檔保存在瀏覽器本地端（`localStorage`）。

## 1. 建立 Firebase 專案

1. 使用您的 Google 帳號前往 <https://console.firebase.google.com>。
2. 點擊 **建立專案** → 輸入專案名稱（例如 `pokesurvivor`）。此專案 ID 會顯示在「透過 Google 登入」的彈出視窗中，建立後無法變更。
3. Google Analytics（分析）：**建議關閉**（本遊戲不需要資料追蹤）。

## 2. 啟用帳號驗證（Authentication）

1. 點擊左側選單 → **建構 (Build) → Authentication** → **開始使用**。
2. 進入 **登入方式 (Sign-in method)** 分頁：
   - **電子郵件/密碼** → 啟用 → 儲存。
     （遊戲採用「使用者名稱 + 密碼」註冊，系統底層會自動轉換為虛擬內部信箱，絕不會對外發送任何實體郵件。）
   - **Google** → 啟用 → 選擇專案的公開支援電子郵件 → 儲存。

## 3. 建立 Firestore 資料庫

1. 進入 **建構 → Firestore Database** → 點擊 **建立資料庫**。
2. 伺服器位置：建議選擇歐洲地區（如 `eur3` 或 `europe-southwest1`），確認後即無法更改。
3. 安全性規則請先選擇 **以正式版模式啟動 (production mode)**。
4. 點擊 **規則 (Rules)** 分頁：清空編輯器中的預設內容，將本專案中的 [`firestore.rules`](../firestore.rules) 完整內容貼上，點擊 **發布 (Publish)**。

   > [!IMPORTANT]
   > 這些安全性規則確保每位玩家只能讀取、寫入與刪除**屬於自己的存檔**。若未配置這些規則，存檔將可能被未經授權竄改。

### 3b. 建立 Realtime Database（用於組隊房間與多人連線）

1. 進入 **建構 → Realtime Database** → 點擊 **建立資料庫** → 位置建議選擇 **比利時 (europe-west1)** → 以鎖定模式建立。
2. 點擊 **規則 (Rules)** 分頁：將本專案中的 [`database.rules.json`](../database.rules.json) 完整內容貼上並儲存。
3. 複製上方顯示的資料庫 URL（例如 `https://...firebasedatabase.app`），準備填入 `firebase-config.js` 的 `databaseURL` 欄位中。

也可以使用 Firebase CLI 一鍵部署兩者的規則與索引：

```bash
npx firebase-tools deploy --only firestore,database --project 你的專案ID
```

## 4. 串接遊戲前端

1. 點擊左上方齒輪圖示 ⚙ → **專案設定**。
2. 在畫面下方的 **您的應用程式** 區塊，點擊網頁圖示 **`</>`**。
3. 輸入應用程式暱稱（例如 `web`）。**不要**勾選 Firebase Hosting。→ 點擊「註冊應用程式」。
4. 系統會顯示 `const firebaseConfig = { ... }` 程式碼區塊，複製括號內的金鑰資訊。
5. 開啟專案中的 [`js/core/firebase-config.js`](../js/core/firebase-config.js)，將 `G.FIREBASE_CONFIG = null;` 替換為：

   ```js
   G.FIREBASE_CONFIG = {
     apiKey: '...',
     authDomain: '...firebaseapp.com',
     projectId: '...',
     storageBucket: '...',
     messagingSenderId: '...',
     appId: '...',
     databaseURL: 'https://...' // 填入 Realtime Database URL
   };
   ```

   > [!NOTE]
   > 這些前端設定參數**並非機密金鑰**：Firebase 本身設計即為公開嵌入前端網頁中。真正保護資料安全的是步驟 3 中設定的資料庫安全規則。

## 5. 授權網域設定

前往 **Authentication → 設定 → 授權網域 → 新增網域**：

- `jpter78.github.io`（若發布於 GitHub Pages）
- `localhost`（用於本機端開發測試，部分新專案預設未包含）。

若未加入此授權，點擊「透過 Google 登入」時會彈出*未授權網域 (unauthorized domain)* 錯誤。

## 6. 安全建議：限制 API 金鑰來源

前往 <https://console.cloud.google.com/apis/credentials>（選擇相同專案），點擊 **Browser key** → *應用程式限制* → 選擇 *網站* 並新增：

- `https://jpter78.github.io/*`
- `http://localhost:*`

如此一來，其他人便無法在未經授權的第三方網域調用您的 API 金鑰。

## 本機測試

雲端存檔功能需透過 Web 伺服器載入（直接透過雙擊檔案 `file://` 打開將無法正常觸發）：

```bash
python3 -m http.server 8000
```

瀏覽器打開 <http://localhost:8000>。在登入畫面中若出現 **透過 Google 登入** 按鈕以及 *"您的進度將儲存於雲端"* 提示即代表設定成功。
遊戲介面右上角使用者名稱旁的小方塊狀態燈：
- **綠燈**：已成功儲存至雲端
- **黃燈**：正在上傳同步中
- **紅燈**：離線或連線中斷

## 免費配額說明

Firestore 免費額度：每日約 50,000 次讀取、20,000 次寫入與 1 GB 儲存空間。本遊戲架構針對配額消耗進行了極致優化：

| 操作情境 | 讀取次數 | 寫入次數 |
|---|---|---|
| 查看單一天梯分頁 | 1 次（若在 10 分鐘內查看過則為 0 次，走快取） | 0 |
| 未入前 50 名時查看個人排位 | 該瀏覽器首次查看為 1 次 | 0 |
| 完成一場探險結算 | 約 3 次 | 約 3~9 次（僅在打破個人紀錄時寫入） |
| 存檔上傳同步 | 最多每 30 秒 1 次 | 最多每 30 秒 1 次 |
| 常駐連線在線狀態 | 0 | 每 10 分鐘 1 次心跳包 |
| 打開好友或房間列表 | 每名好友 1 次（有效快取 1 分鐘） | 0 |

Realtime Database：**支援 100 條同時連線**與每月 10 GB 流量。
只有**進入組隊房間或正在進行多人合作**的玩家才會建立連線（好友在線狀態與邀請函皆走 Firestore），因此 100 條連線配額僅計算當下正在連線闖關的玩家。大部分連線走 WebRTC P2P 直連不消耗資料庫頻寬，僅在 P2P 穿透失敗時走中繼轉發。
即使免費額度當日耗盡，Firebase 僅會暫時停止超出請求至隔日重置，**絕不會自動產生額外扣款**（除非手動綁卡升級付費方案）。

## 本地開發模擬器（Firebase Emulator）

若不想使用線上真實專案測試，可啟動本地模擬器（需 Java 21）：

```bash
npx firebase-tools emulators:start --project demo-ps --only auth,firestore,database
```

並在 `firebase-config.js` 中配置相應的 `emulator: true` 參數。
