---
title: MMBCU 158 DIY 保養知識庫目錄索引
category: index
last_updated: 2026-11-11
description: LLM Wiki 全域內容目錄與雙向實體索引矩陣
---

# MMBCU 158 DIY 保養知識庫總目錄 (Index)

歡迎來到 **SYM MMBCU 158 曼巴跑旅** 的 DIY 保養知識庫。本知識庫基於 **Karpathy LLM Wiki** 模式構建，所有知識由 AI 代理人持續編譯、維護與驗證，具備高精確度的扭力數據、耗材規格、標準 SOP 與交叉引用體系。

---

## 1. 車輛與核心零件實體 (Entities)

涵蓋 MMBCU 158 車身結構、潤滑油品與維修工具的基礎事實定義：

| 實體頁面 | 核心摘要 | 關鍵規格 |
| :--- | :--- | :--- |
| [[MMBCU 158 車款規格]](entities/mmbcu-158.md) | 車系技術架構、水冷單缸 4V 引擎與油道配置 | 機種代號 TDA16T1 / 158c.c. / 水冷 |
| [[四行程引擎機油]](entities/engine-oil.md) | 機油等級規範、黏度指標與油尺檢測法 | 10W-40 / 10W-30 / API SL+ / JASO MB |
| [[變速箱齒輪油]](entities/gear-oil.md) | 齒輪油黏度、更換容量與滑牙防護機制 | 85W-140 / 100~120 c.c. / 10mm 螺栓 |
| [[工具與耗材清單]](entities/tools-and-consumables.md) | DIY 必備手工具尺寸、扭力工具與華司墊片 | 12mm/17mm/10mm 套筒 / M12、M8 鋁墊片 |

---

## 2. 標準作業程序指南 (Guides / SOP)

經由手冊與車友實務相互印證的標準作業流程：

| SOP 指南 | 難易度 | 建議耗時 | 核心重點 |
| :--- | :--- | :--- | :--- |
| [[機油更換完整教學]](guides/engine-oil-change.md) | ★☆☆☆☆ | 15~20 分鐘 | 12mm 側螺栓洩油、17mm 濾網清潔、油位校正、儀表重設 |
| [[齒輪油更換完整教學]](guides/gear-oil-change.md) | ★☆☆☆☆ | 10 分鐘 | 雙螺栓拆卸防呆、重力洩放、扭力防滑牙控制 |

---

## 3. 數據速查與實作紀錄 (Records)

方便車友在車庫手持手機隨查隨用的備忘表格與保養歷史：

- [[規格與扭力速查備忘表]](records/torque-and-capacity-cheatsheet.md)：彙整全車換油容量、洩油螺栓規格、標準扭力範圍與墊片尺寸。
- [[DIY 保養歷史履歷簿]](records/maintenance-log.md)：結構化記載保養日期、里程、使用材料與車況備註。

---

## 4. 維護日誌與來源參照 (Maintenance & Sources)

- [[LLM Wiki 變更日誌]](log.md)：記錄知識庫所有 Ingest / Query / Lint 歷程。
- **原始資料庫 (Raw Sources)**：
  - `raw/sym-mmbcu-manual-excerpt.md`：三陽原廠技術手冊規格
  - `raw/diy-oil-change-notes.md`：論壇車友實務經驗與盲點排查
