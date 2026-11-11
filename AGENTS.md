# AGENTS.md - MMBCU 158 DIY 保養知識庫維護與開發規範

> 本文件為此專案的**核心規範設定檔（The Schema）**，依據 Andrej Karpathy 所倡導的 **LLM Wiki 模式**設計。
> 任何接手此專案的 AI 代理人（Agent）必須嚴格遵守本文件定義的架構、檔案命名、維護操作（Ingest / Query / Lint）與網頁前端呈現規範，以確保知識庫在持續演進擴展時維持高度一致性、交叉連結完備性與技術準確性。

---

## 1. 專案定位與核心目標

- **專案名稱**：SYM MMBCU 158 DIY 保養知識庫（曼巴蛇跑旅車系）
- **核心目標**：
  1. **建立持久累積知識庫**：打破傳統 RAG「問完即忘、每次重新檢索拼湊」的缺點，將機油、齒輪油、濾網、耗材規格及保養履歷整理為結構化、相互鏈接的 Markdown 知識庫。
  2. **標準化 DIY 維修 SOP**：提供精確的工具規格（套筒尺寸）、原廠手冊交換容量、鎖緊扭力值、安全檢查點與防呆預防措施（如防止鋁合金齒輪箱滑牙、防爆油封）。
  3. **自動化同步與前端展現**：知識庫內容直接對應並支援靜態網頁（GitHub Pages）展示，具備可互動的保養紀錄計算器與規格速查表。
  4. **可延伸與可維護性**：讓未來的 AI 或協作者能依照同一套規範，無縫擴展空濾、水箱水、煞車油、傳動等其他保養項目。

---

## 2. 三層架構定義（Three-Layer Architecture）

知識庫嚴格劃分為三個獨立層次，權限與職責不可混淆：

```text
mmbcu-maintenance-wiki/
├── AGENTS.md               # [第三層：The Schema] 規範與行為準則（本文件）
├── README.md               # 專案說明與 GitHub Pages 部署指引
├── index.html              # [展示層] 現代化響應式知識庫網頁（支援 GitHub Pages 與 Canvas）
│
├── raw/                    # [第一層：Raw Sources] 原始資料層（唯讀不可改，Source of Truth）
│   ├── sym-mmbcu-manual-excerpt.md
│   └── diy-oil-change-notes.md
│
└── wiki/                   # [第二層：The Wiki] 持久知識庫層（AI 完全負責維護與編譯）
    ├── index.md            # 全域內容目錄與交叉引用矩陣
    ├── log.md              # 追蹤所有 Ingest / Query / Lint 變更的時間軸日誌
    ├── entities/           # 零件、車型、耗材與工具等名詞實體頁
    │   ├── mmbcu-158.md
    │   ├── engine-oil.md
    │   ├── gear-oil.md
    │   └── tools-and-consumables.md
    ├── guides/             # 逐步實作指南與標準作業程序（SOP）
    │   ├── engine-oil-change.md
    │   └── gear-oil-change.md
    └── records/            # 規格數據表與保養紀錄範本
        ├── torque-and-capacity-cheatsheet.md
        └── maintenance-log.md
```

### 第一層：原始資料層（Raw Sources - `raw/`）
- **性質**：唯讀（Immutable）。
- **職責**：存放收集到的原廠車主手冊摘錄、維修手冊截圖文字、論壇車友實測筆記、原廠料號清單等。
- **規則**：AI **絕不可直接修改** `raw/` 中的文件，所有變更與延伸必須沉澱到 `wiki/` 層。

### 第二層：持久知識庫層（The Wiki - `wiki/`）
- **性質**：由 AI 代理人全權編譯、更新與維護的 Markdown 庫。
- **職責**：
  - 提煉原始資料中的關鍵數據與流程。
  - 維護雙向鏈接（Cross-references）、標記矛盾（Contradictions）、填補資料空白。
  - 每當吸收新來源時，同步更新實體頁面（Entities）、指南頁面（Guides）與全域目錄（`index.md`）。

### 第三層：規範設定檔（The Schema - `AGENTS.md`）
- **性質**：AI 的系統指示與專案工作憲章。
- **職責**：定義檔案命名、目錄責任、Markdown 格式、前端設計原則與 AI 操作指令流程。

---

## 3. 核心維護操作流程（Operations SOP）

AI 在協助維護知識庫時，必須執行以下三大核心操作：

### 3.1 攝取新來源（Ingest）
當使用者上傳或貼入新的維修筆記、手冊內容或車友經驗時，AI 必須依序執行：
1. **閱讀與比對**：仔細比對新資料與現有 `wiki/` 內容（例如：新的洩油螺栓扭力值是否與原手冊記載不同）。
2. **沉澱原始檔**：將新資訊妥善歸檔至 `raw/<descriptive-name>.md`。
3. **擴增或修訂 Wiki**：
   - 若為新零件/主題，在 `wiki/entities/` 建立新頁面。
   - 若為新作業流程，在 `wiki/guides/` 建立 SOP 頁面。
   - 若發現數據矛盾（如機油量手冊寫 1000ml 但車友指出日常重力洩油 800ml 即可），在對應實體與指南頁面明確標註「原廠規範 vs 實務建議」，絕不草率覆蓋。
4. **更新全域索引**：在 `wiki/index.md` 加入新頁面的連結與單行摘要。
5. **寫入日誌**：在 `wiki/log.md` 頂端追加標準格式記錄：
   ```markdown
   ## [YYYY-MM-DD] ingest | 新增標題或來源名稱
   - 來源檔案：raw/xxx.md
   - 異動頁面：wiki/entities/xxx.md, wiki/guides/xxx.md
   - 核心摘要：提煉了哪些關鍵數據或流程。
   ```
6. **同步更新前端**：若變更影響主要 SOP 或速查數據，同步更新根目錄之 `index.html`。

### 3.2 深度查詢與知識沉澱（Query）
當使用者提出複雜保養問題（例：「更換機油時發現漏斗太短卡到空濾，且螺栓差點滑牙該怎麼辦？」）：
1. AI 先檢索 `wiki/index.md` 與對應的 `wiki/guides/` 和 `wiki/entities/`。
2. 結合精確規格提供圖文並茂的解決方案（建議工具長度、扭力扳手規格、防呆建議）。
3. **關鍵原則（回饋沉澱）**：若該次問答產出了高價值的排除指南或技巧，AI 應主動將此成果編譯為 Wiki 的新增章節或修訂條目，並記錄於 `wiki/log.md` 中，讓知識持續複利（Compounding）。

### 3.3 知識庫健康檢查（Lint）
使用者可隨時要求 AI 對知識庫執行健康檢查，AI 應自動排查：
- **孤立頁面（Orphans）**：是否存在未被 `wiki/index.md` 或其他頁面引用的檔案。
- **失真矛盾（Contradictions）**：檢查各頁面間的數據（容量、扭力、工具規格）是否互相矛盾。
- **待填補空白（Gaps）**：列出尚未完整說明的延伸項目（例如尚未撰寫的機油濾網清洗扭力、副廠磁石螺絲建議）。
- **日誌一致性**：確認 `wiki/log.md` 之紀錄與實際檔案吻合。

---

## 4. 檔案與目錄命名規範

1. **命名原則**：
   - 檔案名稱全小寫，單字間以連字號 `-` 分隔（kebab-case）。
   - 禁止使用空格、特殊符號或全形字元（例如：`engine-oil-change.md`，不可為 `Engine Oil Change.md`）。
2. **目錄分類標準**：
   - `wiki/entities/`：名詞、零件、耗材、車型基本概念（例：`engine-oil.md`, `mmbcu-158.md`）。
   - `wiki/guides/`：動詞導向的步驟教學、標準作業流程（例：`engine-oil-change.md`）。
   - `wiki/records/`：結構化數據、對照表、備忘單、紀錄簿（例：`torque-and-capacity-cheatsheet.md`）。
3. **Markdown 頁面格式範本**：
   每一份 wiki 文件均應具備 YAML Frontmatter：
   ```markdown
   ---
   title: 頁面完整繁體中文名稱
   category: entities | guides | records
   last_updated: YYYY-MM-DD
   tags: [mmbcu, maintenance, diy, ...]
   related_pages:
     - ../entities/xxx.md
     - ../guides/xxx.md
   ---

   # 頁面標題

   > 單行核心結論或定位說明。

   ## 核心技術規格 / 前置要求
   ...
   ## 詳細作業流程 / 說明
   ...
   ## 注意事項與防呆檢查
   ...
   ## 關聯實體與延伸參考
   - [[關聯頁面名稱]](../entities/xxx.md)：關聯說明
   ```

---

## 5. MMBCU 158 核心保養規格（基準事實 Truth Anchor）

AI 在維護與回答本專案內容時，必須以此基準事實為依據，嚴禁虛構數據：

| 保養項目 | 原廠更換容量 (Exchange) | 分解/總容量 | 原廠指定黏度 / 規格 | 拆卸工具尺寸 | 建議鎖緊扭力值 | 原廠建議週期 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **引擎機油** | **1000 cc** (日常重力洩放約 800~900 cc，以油尺為準) | 1050 cc | SAE 10W-40 / 10W-30 (API SL/SM/SN 以上，JASO MB/MA2) | 12mm 六角套筒 (側邊洩油螺栓)<br>17mm 外六角套筒 (底濾網螺蓋) | 12mm 螺栓：**1.2 ~ 1.5 kgf-m** (12~15 N-m)<br>17mm 濾網蓋：**2.0 ~ 2.5 kgf-m** (20~25 N-m) | 初次 300 km<br>後續每 1,000 ~ 2,000 km |
| **變速箱齒輪油** | **100 cc ~ 120 cc** (1 支條裝，嚴禁過量加注) | 約 130 cc | SAE 85W-140 或 10W-30 原廠齒輪油 | 10mm 六角套筒/梅花扳手 (注油與洩油螺栓) | **1.0 ~ 1.4 kgf-m** (10~14 N-m，極易滑牙，嚴禁硬鎖) | 初次 300 km<br>後續每 3,000 ~ 5,000 km |
| **洩油華司墊片** | 機油：M12 鋁/銅墊片<br>齒輪油：M8 鋁/銅墊片 (各 1 片) | - | 建議每次更換油品時一併更新墊片，以防滲漏 | - | 隨螺栓扭力壓平緊固 | 每次換油檢查更換 |

---

## 6. 網頁設計與前端規則（Web Application Specification）

專案根目錄之 `index.html` 必須符合以下要求，確保兼具可直接單檔打開（Canvas 相容）與發布至 GitHub Pages 的特質：

1. **視覺風格與主題（Theme & Aesthetics）**：
   - 品牌色彩：呼應 MMBCU 曼巴蛇紋意象，主色採用深沉洗鍊的科技墨綠（`#064e3b` / `#022c22`）結合深色碳纖維背景（`#0f172a`），輔助點綴警示金橘（`#f59e0b` / `#d97706`）與純白高對比文字。
   - 排版與視覺階層：清楚的卡片邊框、圓角（`rounded-xl`）、精緻的毛玻璃陰影（backdrop blur）與流暢的頁籤切換動態效果。
2. **功能模組規劃**：
   - **頁籤 1：知識庫總覽（Wiki Index & Graph）**：展現 LLM Wiki 的架構理念、實體關聯圖譜與分類目錄索引。
   - **頁籤 2：機油更換 SOP（Engine Oil Guide）**：圖文並茂拆解「準備工序 → 洩放機油 → 濾網清潔 → 墊片扭力復歸 → 注入新油與油尺確認 → 儀表歸零」全流程。
   - **頁籤 3：齒輪油更換 SOP（Gear Oil Guide）**：詳細引導「拆注油螺絲 → 拆洩油螺絲 → 徹底排空 → 鎖回洩油螺栓 → 注入 100~120ml → 鎖回注油螺栓」與防爆油封注意事項。
   - **頁籤 4：規格與扭力速查表（Cheatsheet）**：表格化即時搜尋與高對比數據卡片。
   - **頁籤 5：DIY 保養紀錄簿（Maintenance Tracker）**：具備互動計算器，可讓車主輸入更換日期、當前里程、使用機油/齒輪油品牌與備註，自動計算下次建議保養里程，支援儲存於瀏覽器 `localStorage`，可新增、刪除及匯出資料。
   - **頁籤 6：LLM Wiki 維護日誌（Wiki Changelog）**：即時載入 `wiki/log.md` 變更記錄，展現 LLM Wiki 持續演進的生命週期。
3. **技術實現原則**：
   - 純單一檔案（Single self-contained HTML），採用 CDN 載入 Tailwind CSS 與 Feather Icons / Lucide Icons，無外部本地模組相依。
   - 針對行動裝置優化，適應車友在車庫、騎樓手持手機一邊對照一邊保養的場景。

---

## 7. AI 代理人後續修改與擴充指引（How AI Continues This Project）

當使用者要求：「**請幫我新增 MMBCU 的『空氣濾清器（空濾）更換教學』**」時，接手的 AI 請遵循標準五步驟：

1. **查閱現有結構**：確認是否有相關實體。在 `wiki/entities/air-filter.md` 建立空濾實體，記錄更換週期（每 5,000 km 清潔或更換）、十字螺絲規格（外蓋 6 顆螺絲）。
2. **撰寫實作指南**：在 `wiki/guides/air-filter-change.md` 撰寫圖文 SOP，包含拆卸順序、肥腸管檢查與廢油管排空。
3. **維護索引矩陣**：在 `wiki/index.md` 的「耗材保養指南」類別加入此指南連結。
4. **追加變更日誌**：在 `wiki/log.md` 寫入 `## [YYYY-MM-DD] ingest | 新增空氣濾清器更換教學`。
5. **增添網頁介面**：在 `index.html` 增補該項目之步驟卡片或在速查表中增加空濾檢視列。

此規則確保任何一位 AI 都能像經驗豐富的資深維護者一樣，維持整個專案的嚴謹度與生命力。
