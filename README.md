# SYM MMBCU 158 DIY 保養知識庫 (LLM Wiki Architecture)

> 🏍️ 本專案參考 **Andrej Karpathy 倡導的 LLM Wiki 設計模式**，專門為三陽工業（SYM）旗艦跑旅速克達 **MMBCU 158（曼巴）** 打造的持續性 DIY 保養知識庫。
> 涵蓋引擎機油更換、齒輪油更換、機油濾網清潔、耗材與扭力速查、以及可互動之車主自主保養履歷追蹤。

---

## 🌟 核心特色

1. **三層式持久架構（Three-Layer Architecture）**：
   - **原始資料層 (`raw/`)**：收錄原廠修護手冊與車友論壇筆記，作為只讀不改的真實數據源（Source of Truth）。
   - **持久知識庫 (`wiki/`)**：由 AI 全權維護與編譯，包含目錄索引 (`index.md`)、操作時間軸 (`log.md`)、實體定義 (`entities/`) 與標準作業指南 (`guides/`)。
   - **規範設定檔 (`AGENTS.md`)**：提供接手 AI 明確的命名慣例、Ingest/Query/Lint SOP 與前端同步規則，確保知識庫持續複利成長。
2. **消解數據矛盾（Truth Anchoring）**：
   - 清楚對比「原廠手冊 1000ml」與「社群實務 800~850ml」之容量迷思，詳細解釋過量加注之曲軸箱阻力與空濾噴油風險。
   - 標記齒輪箱 10mm 鋁合金螺孔之滑牙高風險，制定「10~14 N-m 微扭力」與「先開注油孔、再開洩油孔」之防呆原則。
3. **即時互動式單頁網頁 (`index.html`)**：
   - 支援直接雙擊瀏覽、Canvas 預覽與 GitHub Pages 靜態代管。
   - 包含知識庫總覽、機油更換 SOP、齒輪油更換 SOP、即時搜尋扭力速查表、以及支援本機儲存與 CSV 匯出之保養紀錄簿。

---

## 📁 檔案與目錄結構

```text
.
├── AGENTS.md               # 核心規範文件（The Schema：定義 AI 維護行為與擴展準則）
├── README.md               # 專案簡介與部署說明
├── index.html              # 互動式知識庫前端網頁（可直接透過 GitHub Pages 上線）
│
├── raw/                    # 第一層：原始資料層（Immutable，唯讀）
│   ├── sym-mmbcu-manual-excerpt.md    # 原廠車主手冊摘錄與技術數據
│   └── diy-oil-change-notes.md        # 車友論壇實務經驗與盲點排查
│
└── wiki/                   # 第二層：持久知識庫層（由 AI 自動編譯維護）
    ├── index.md            # 全域知識庫目錄與雙向索引
    ├── log.md              # 追蹤 Ingest / Query / Lint 的時間軸日誌
    ├── entities/           # 零件、車款與耗材實體頁
    │   ├── mmbcu-158.md               # MMBCU 158 車型技術諸元
    │   ├── engine-oil.md              # 四行程機油規格與油量標準
    │   ├── gear-oil.md                # 變速箱齒輪油規格與齒輪箱結構
    │   └── tools-and-consumables.md   # DIY 保養必備手工具與耗材清單
    ├── guides/             # 標準作業程序指南 (SOP)
    │   ├── engine-oil-change.md       # 機油更換標準 SOP (含濾網清洗與儀表歸零)
    │   └── gear-oil-change.md         # 齒輪油更換標準 SOP (防呆順序與防滑牙)
    └── records/            # 規格數據與保養紀錄簿
        ├── torque-and-capacity-cheatsheet.md  # 規格與扭力速查備忘表
        └── maintenance-log.md                 # 車主自主保養履歷記錄簿
```

---

## 🚀 如何上傳至 GitHub 並啟用 GitHub Pages

### 步驟 1：本機 Git 初始化與提交
在專案目錄下打開終端機（Terminal）執行：

```bash
git init
git add .
git commit -m "feat: init MMBCU 158 DIY maintenance knowledge base with Karpathy LLM Wiki pattern"
```

### 步驟 2：推送到 GitHub
登入 GitHub 建立一個新的公開儲存庫（Repository，例如 `mmbcu-diy-wiki`），然後將本地庫關聯並推送：

```bash
git branch -M main
git remote add origin https://github.com/<你的GitHub帳號>/mmbcu-diy-wiki.git
git push -u origin main
```

### 步驟 3：開啟 GitHub Pages 網頁成果
1. 前往 GitHub 該 Repository 頁面，點選頂部選單的 **Settings**（設定）。
2. 在左側選單點選 **Pages**。
3. 在 **Build and deployment** 底下的 **Branch** 選擇 `main`，資料夾選擇 `/ (root)`，點選 **Save**。
4. 等待約 1~2 分鐘，頁面上方即會出現你的公開網頁網址：
   `https://<你的GitHub帳號>.github.io/mmbcu-diy-wiki/`
5. 打開該網址即可正常瀏覽與操作互動保養知識庫！

---

## 🤖 如何讓 AI 依據本專案規範繼續擴展？

當你未來想增加「**MMBCU 空氣濾清器（空濾）更換教學**」或「**火星塞更換教學**」時，只需將需求與 `AGENTS.md` 提示給 AI：

> 「請依照本專案 AGENTS.md 的規範，為 MMBCU 158 新增空氣濾清器（空濾）的實體頁、SOP 指南，並同步更新 wiki/index.md、wiki/log.md 以及前端 index.html。」

AI 閱讀 `AGENTS.md` 後，將精確遵循：
1. 在 `wiki/entities/air-filter.md` 定義原廠規格與更換週期（每 5,000 km）。
2. 在 `wiki/guides/air-filter-change.md` 撰寫外蓋 6 顆十字螺絲、空濾箱廢油管排空之 SOP。
3. 於 `wiki/index.md` 建立雙向鏈接。
4. 於 `wiki/log.md` 追加標準格式日誌 `## [YYYY-MM-DD] ingest | 新增空氣濾清器更換教學`。
5. 於 `index.html` 增添對應卡片與速查表列。
# mmbcu-diy-wiki
# mmbcu-diy-wiki
