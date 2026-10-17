# PokéSurvivor · 寶可夢不可思議的迷宮

結合《寶可夢不可思議的迷宮》（Pokémon Mystery Dungeon）風格與 Survivor.io、Megabonk 玩法的寶可夢彈幕倖存者（Bullet-Heaven）遊戲。具備 Roguelike 元素、**收錄第 1 至第 9 世代共 966 隻寶可夢**、異色寶可夢（Shiny）與各地區專屬轉蛋機，並配有為本作原創譜寫的音樂與音效。

採用純 HTML + CSS + 原生 Vanilla JavaScript 搭配 Canvas 2D 打造。無任何構建步驟、零外部套件依賴，且**完全於本機端運行**：無需網路連線即可遊玩。

> **加密貨幣免責聲明 / Crypto disclaimer**
>
> PokéSurvivor **沒有任何官方加密貨幣、代幣或 NFT**，且本人從未發行過任何相關項目。任何使用本遊戲名稱的代幣均為第三方所建立。**本人不承擔任何購買、出售或投資的法律責任**，此處任何內容均不構成投資建議。本遊戲完全免費，且將永久保持免費。
>
> PokéSurvivor has **no official cryptocurrency, token or NFT**, and I have not created any. Any token using the game's name was created by third parties. **I take no responsibility for any purchase, sale or investment** you make with them, and nothing here is investment advice. The game is free and will stay free.

## 開始遊玩

雙擊開啟 `index.html`。可建立帳號或以訪客身分登入，完成性格測驗後即可進入迷宮展開探險。

| 按鍵 | 操作說明 |
|---|---|
| `W` `A` `S` `D` / 方向鍵 | 移動 |
| `1` `2` `3` `4` | 選擇當前啟用的招式 |
| `Q` / `E` | 輪替切換招式 |
| `Esc` / `P` | 暫停 |

**手機或平板操作**（橫向螢幕）：在螢幕任意位置按住並拖曳即可移動（虛擬搖桿會出現在手指觸控處），點擊底部招式圖示切換招式，點擊右上角按鈕暫停遊戲。可透過瀏覽器「加入主畫面」安裝為獨立 App。在**設定**中可調節音效、畫面品質（支援自動調節）、傷害數字顯示、畫面震動幅度、FPS 幀率顯示與帳號管理。

寶可夢會自動使用當前啟用的招式進行攻擊（或透過**招式連結**：每擊敗一隻傳說的寶可夢，即可融合招式，單一欄位最多可同時施展 4 個招式）。場景中的岩石、寶箱與高草叢均可被破壞；牆壁與樹木能阻擋敵方遠程投射物；水域會降低移動速度，岩漿則會造成灼傷傷害（對敵人同樣有效）。

地圖上隨機分佈著**力量祭壇**、可恢復生命值的**泉水**、**上鎖寶箱**（需在其身旁停留數秒以解鎖）以及能重創敵人的**陷阱**。有時地面會出現**時空裂縫**：踏入裂縫將進入獨立競技場，與**傳說的寶可夢**展開單挑對決。

遊戲中具備完整的**屬性相剋表**（例如泡沫光線對小火龍能造成更高傷害），場景也會隨機變換**天氣**（晴天、雨天、沙暴、雪天、濃霧），進一步增強或削弱相應屬性的招式威力。

每日與每週均有**任務**可獲取轉蛋券與金幣；**成就**系統獎勵獎章與專屬稱號；**個人資料**展示你最喜愛的 3 隻寶可夢、歷史最佳紀錄與獎章。探險途中可拾取各類**道具**（如剩飯、講究頭帶等），且招式可在升級至最高等級時**進化**。敵方寶可夢擁有各自的行為 AI：部分會提供治療、架設護盾、召喚同伴，或在施放強力攻擊前於地面顯示預警區域。

各寶可夢的**能力強化**獨立計算。**轉蛋**消耗透過遊戲獲得的**轉蛋券**（擊敗部分普通寶可夢獲取 1 張，擊敗 Boss 與傳說的寶可夢獲取 10 張），並於像素風格的拉霸機上進行抽取。

野生敵人有 1/4096 的機率以**異色（Shiny）**形態登場：身上伴隨閃爍光芒，且螢幕邊緣會顯示箭頭導引其方位；將其擊敗即可直接收服。在轉蛋卡池中，異色寶可夢的抽取機率為 1/100。在收藏庫中，可自由選擇以一般或異色版本作為同行夥伴。

首次點擊頁面後將自動播放音樂（遵循現代瀏覽器禁止無互動自動播放的音訊規範）。可在右上角揚聲器按鈕或暫停選單中調節音量與靜音。

存檔資料預設儲存於瀏覽器的 `localStorage` 中；若清除網站快取與資料，本機存檔將會遺失。

在 `videos/` 目錄中收錄了遊戲實機演示影片（含配樂）：包含頭目戰鬥、捕獲野外異色寶可夢以及轉蛋抽中異色的過程。

## 雲端存檔與線上功能

- **資料庫（Firebase）：** 支援以使用者名稱與密碼或 Google 帳號登入，即可跨裝置接續遊玩進度。詳細設定請參閱 [docs/FIREBASE.md](docs/FIREBASE.md)。（未設定 Firebase 時，遊戲將純粹於本機瀏覽器中存檔）。
- **多人連線合作（最多 4 人）與排行榜：** 登入雲端帳號後，可使用**好友**功能（依暱稱搜尋加入好友、邀請進入隊伍房間）以及**排行榜**（單人或組隊存活時間：涵蓋今日、本週、本月、年度與歷史排行，並支援好友篩選與依寶可夢種類篩選）。
- **免費發布至 GitHub Pages：** 請參閱 [docs/PUBLICAR.md](docs/PUBLICAR.md)。
- 若同時在多台裝置遊玩或曾離線遊戲，存檔將會**智慧合併**：圖鑑與收藏自動取聯集（絕不遺失任何已獲得的寶可夢或異色版本），數值強化與成就統計保留最高數值，金幣數量與選用夥伴則採用最新時間戳記。多端修改支援即時同步。

## 授權條款

程式碼自 2026 年 10 月 14 日起採用 [PolyForm Noncommercial 1.0.0](LICENSE-NONCOMMERCIAL.md) 授權：任何人均可自由使用、研究與修改，但**嚴禁任何商業營利用途**（包含販售遊戲本體或於遊戲內進行任何收費）。commit `b297089`（含）以前的版本基於 MIT 授權釋出。精靈圖資源（CC BY-NC 4.0）、字型（OFL）以及「寶可夢」相關商標權利均受各自條款約束：詳見 [LICENSE](LICENSE) 與 [CREDITS.md](CREDITS.md)。本專案為非營利粉絲自製同人作品。

**唯一官方網站為 [pokesurvivor.com](https://pokesurvivor.com)。** 本遊戲現在與未來皆完全免費：任何向您索取費用的平台或個人均非官方。

## 專案結構

```
index.html              頁面視圖與資源載入順序（發布版）
css/style.css           不可思議的迷宮風格 UI 樣式
assets/pokemon/{dex}/   PMD 精靈圖幀表（包含 Idle、Walk、Attack、Shoot、Charge、Hurt、Faint 等動作）
assets/pokemon/{dex}/s/ 對應的異色版本精靈圖
assets/fonts/           Pixelify Sans 像素字型 (OFL) 及經數字微調後的版本 (tools/make_font.py)
js/
  net/      社群功能（暱稱、好友、組隊房間）、連線傳輸（WebRTC 與備用中繼）、多人連線與排行榜
  core/     工具函式(util)、存檔系統(db)、音效合成器(audio)、輸入控制(input)、動畫控制器(sprites)、攝影機(camera)
  data/     寶可夢圖鑑(pokedex)*、精靈圖元數據(sprites-meta)*、招式(moves)、初始夥伴(starters)、敵人(enemies)、升級項目(upgrades)、音樂樂譜(music)
  world/    地圖瓦片生成(tiles)、世界場景與碰撞偵測(world)
  entities/ 玩家角色(player)、敵人(enemy)、遠程彈道(projectile)、拾取物(pickup)、特效(fx)
  systems/  戰鬥系統(combat)、怪物生成(spawner)、升級卡牌(levelup)、轉蛋(gacha)
  ui/       像素圖示(icons)、UI 底層、HUD 抬頭顯示、登入、測驗、選單、轉蛋介面、探險結算、社群與排行介面
  game.js   主遊戲迴圈與狀態機流程
tools/      自動化腳本（用於重新產生標註 * 之資料檔案）
```

標記 `*` 代表自動產生的檔案。若需重新擷取與產生（僅首次需要，需 Python 3 + Pillow 並連線至網際網路）：

```bash
python tools/fetch_sprites.py    # 抓取一般與異色精靈圖並生成 js/data/sprites-meta.js
                                 # (約需 20 分鐘；可中斷並隨時繼續：進度快取於 tools/cache/)
python tools/fetch_pokedex.py    # 生成 js/data/pokedex.js
python tools/fetch_credits.py    # 生成 CREDITS.md 貢獻致謝名單
```

## 實作設計與技術細節

- **PMD 精靈圖幀表：** 欄（Columns）為動畫幀數，列（Rows）為 8 個面向方向（Down、DownRight、Right、UpRight、Up、UpLeft、Left、DownLeft——已透過 `-Offsets.png` 像素校驗驗證）。每個動畫擁有不同的畫格尺寸，因此渲染時統一依據從 `-Shadow.png` 提取的**腳底接觸地面基準點（Ground Anchor）**進行錨定對齊；避免攻擊或切換動作時精靈圖發生跳動偏移。
- **座標系統：** 實體的世界座標 `(x, y)` 定位於其**足底中心點**。碰撞判定發生在地面水平面上；遠程飛行道具同樣於該平面進行軌跡運算，繪製時額外附加高度偏移並於正下方投射陰影。
- **世界地圖：** 採用 16×16 瓦片（Tiles）組成的區塊（Chunk）劃分：地圖資料（地形、物件）生成開銷極低，可全域維護；而視覺繪製層則透過直接寫入 `ImageData`（在 Chrome 下每個 Chunk 僅需約 4 ms）進行即時烘焙，且僅針對可視區域內的 Chunk 執行。
- **運算效能：** 每幀動態重建敵人的空間分割網格（Spatial Grid）。即使場上同時存在 420 隻敵人，在 Chrome 瀏覽器下的完整物理模擬與繪製渲染僅需約 3 ms/frame。
- **使用 `.js` 而非 `.json` 儲存資料：** 確保直接以 `file://` 協定雙擊打開檔案時，不會受到瀏覽器跨來源資源共享（CORS）與本機 `fetch()` 安全策略的阻擋。
- **異色（Shiny）處理：** SpriteCollab 的異色圖多為相同線稿搭配不同調色盤；檔案收錄於 `s/` 目錄下，且其中僅 30 隻寶可夢具有相異的幾何幀尺寸（記錄於元數據的 `sa` 欄位）。圖檔一律以精確調色盤之 PNG 格式儲存：畫面無失真，檔案容量減少一半。
- **音效架構：** 採用 Web Audio API 原生音訊合成器，並配備前瞻排程器（提前 150 ms 預先排程音符事件）以及脈衝響應卷積殘響效果。樂譜以自研的文字格式定義於 `js/data/music.js`。音效模組內建最短重複間隔與發聲數上限限制，避免同屏 400 隻敵人同時受到傷害時引發音訊破音或卡頓。
- **像素圖示：** 字元映射表轉為 Canvas 與 Data URL；一律以整數倍率進行縮放，維持乾淨俐落的復古像素風格。

## 鳴謝與素材來源

精靈圖素材來自 [SpriteCollab](https://github.com/PMDCollab/SpriteCollab)，遵循 **CC BY-NC 4.0** 授權條款——僅供非商業使用並保留署名；完整創作者名單請參見 [`CREDITS.md`](CREDITS.md)。
「精靈寶可夢 / 寶可夢（Pokémon）」版權為任天堂（Nintendo）、Creatures Inc. 及 GAME FREAK 所擁有。本專案為非商業性質的寶可夢同人愛好者專案。
