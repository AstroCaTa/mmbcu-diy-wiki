# MMBCU 158 DIY 保養知識庫變更日誌 (Changelog)

> 本日誌為嚴格遞增追加（Append-Only）之時間軸紀錄，記錄所有 Ingest（來源攝取）、Query（查詢沉澱）與 Lint（健康檢查）操作。
> 格式規範：`## [YYYY-MM-DD] action | title`

---

## [2026-11-11] lint | 知識庫完整性與交叉鏈接驗證
- **執行類別**：系統健康檢查 (Lint)
- **檢查項目**：
  - 驗證所有實體頁面（Entities）與指南頁面（Guides）是否存在孤立未鏈接狀況。
  - 核對原廠手冊 1000ml 交換量與車友社群實務 800ml 的差異性標註，確認兩者已於機油實體與更換指南中明確對比，消解矛盾。
  - 確認網頁端數據與知識庫 Markdown 一致。
- **檢查結論**：所有雙向鏈接完整，無孤立頁面，技術參數與防呆提示均已具備。

## [2026-11-11] ingest | 建立 MMBCU 158 齒輪油更換 SOP 與規格速查表
- **來源檔案**：`raw/sym-mmbcu-manual-excerpt.md`, `raw/diy-oil-change-notes.md`
- **產出檔案**：
  - `wiki/entities/gear-oil.md`
  - `wiki/guides/gear-oil-change.md`
  - `wiki/records/torque-and-capacity-cheatsheet.md`
- **核心沉澱**：
  - 提煉齒輪油容量規範（100~120cc），特別警告雙倍灌入易導致後軸油封衝破風險。
  - 確立 10mm 螺栓緊固扭力（1.0~1.4 kgf-m），嚴防鋁合金齒輪箱滑牙。
  - 確立「先開注油螺栓、再開洩油螺栓」之防呆流程。

## [2026-11-11] ingest | 建立 MMBCU 158 基礎車款實體與機油保養 SOP
- **來源檔案**：`raw/sym-mmbcu-manual-excerpt.md`, `raw/diy-oil-change-notes.md`
- **產出檔案**：
  - `wiki/entities/mmbcu-158.md`
  - `wiki/entities/engine-oil.md`
  - `wiki/entities/tools-and-consumables.md`
  - `wiki/guides/engine-oil-change.md`
  - `wiki/records/maintenance-log.md`
- **核心沉澱**：
  - 定義車型代號 TDA16T1，釐清 12mm 側邊洩油螺栓與 17mm 底濾網外蓋之分工。
  - 明確機油尺「不旋入螺牙」標準測量法。
  - 整合儀表 OIL CHANGE 提示燈 1000km 歸零重設步驟。

## [2026-11-11] init | 專案初始化與 Karpathy LLM Wiki 架構建立
- **變更說明**：
  - 依據 Andrej Karpathy LLM Wiki 模式建立三層架構：Raw Sources (`raw/`)、The Wiki (`wiki/`) 與 The Schema (`AGENTS.md`)。
  - 建立全域索引目錄 `wiki/index.md` 與時間軸日誌 `wiki/log.md`。
