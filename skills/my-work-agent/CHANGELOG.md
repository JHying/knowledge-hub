# Changelog — my-work-agent

所有版本異動依時間倒序排列。

---

## [2.3] — 2026-07-01

### Added
- **SA 角色**：新增獨立 SA（System Analyst）角色，對應 Spec 轉化 stage，取代原 PM 兼任 Spec 轉化的雙重職責

### Changed
- Step 2-SINGLE 角色選單：PM 現在只對應需求企劃；SA 獨立列出對應 Spec 轉化（含 ADR 溝通）；移除 PM 的 stage 追加確認問題
- Step 4 pipeline 文件表：Spec 轉化 角色文件從 `{{role_pm}}` 改為 `{{role_sa}}`，流程文件從 `{{flow_pm}}` 改為 `{{flow_sa}}`
- Step 5-SINGLE 角色對應表：PM → 需求企劃，SA → Spec 轉化（含 ADR 溝通）
- Step 5-PIPELINE Spec 轉化 stage 標注：PM + CONSULTANT → SA + CONSULTANT

---

## [2.2] — 2026-07-01

### Changed
- Step 2-SINGLE 角色選單：補充各角色對應的完整工具呼叫說明（`/update-kb`、`/diagram`、`/code-architect` 等）
- PM 角色新增 stage 確認步驟：選 PM 後追加問「需求企劃」或「Spec 轉化（SA）」，明確對應 pipeline stage
- Step 5-SINGLE：不再只說「按流程文件執行」，改為明確對應 Step 5-PIPELINE 各 stage 執行細節（含所有工具呼叫）；SRE 為例外，依 flow_sre 執行後詢問是否 `/update-kb`
- 單一角色模式統一為 confirm 模式

---

## [2.1] — 2026-07-01

### Changed
- MULTI 模式重新命名為 **PREVIEW**，更清楚傳達「開工前輕量雙視角探索」的用途

---

## [2.0] — 2026-07-01

### Added
- **執行模式選擇**（Step 2）：新增四種模式 — 單一角色 / 部分流程 / 完整流程 / MULTI
- **部分流程**（Step 2-PIPELINE）：使用者指定起始 stage（需求企劃 / Spec 轉化 / Spec-Driven 實作 / Code Review / QA），從該 stage 依序執行至 QA
- **完整流程**：從需求企劃執行至 QA 的全 pipeline
- **per-stage auto / confirm 設定**（Step P2）：每個 stage 可獨立選擇 auto（自動執行）或 confirm（每個決策點與使用者確認）
- **Step 5-PIPELINE**：Pipeline 執行引擎，含各 stage 詳細執行邏輯、ADR 溝通整合、`/update-kb` 觸發時機、`/diagram` 與 `/diagram sync` 執行點、stage 間銜接格式與完成總結輸出

### Changed
- Step 2 原「選擇角色」移至 Step 2-SINGLE，單一角色模式下才顯示
- MULTI 模式從 Step 2 角色選項移至獨立的執行模式選項（選項 4）
- Step 4 新增 pipeline 模式的文件預載表（各 stage 對應角色文件與流程文件）

---

## [1.2] — 2026-06-26

### Changed
- 通用 KB（ADRs / tech-research）改為 index-first 載入：subagent 先讀 `common_KBs/MASTER_INDEX.md`，依 Story 主題僅讀取相關 ADR 分類與 tech-research 筆記，不再全量載入
- `common_KBs/guideline/REVIEW_GUIDE.md` 改為 REVIEWER 必讀，其餘角色依需要載入
- Step 1.5 說明：移除「自動載入」措辭，改為「依 Story 主題按需載入」
- Step 3 動態路徑注入：以 `common_KBs/MASTER_INDEX.md` 取代三個個別路徑
- BACKEND / QA Subagent prompts 必讀文件：以「通用 KB 主索引 + 按需讀取」取代全量載入的三個 common_KBs 項目

## [1.1] — 2026-06-26

### Changed
- 共用規範路徑從 `knowledge/guideline/` 移至 `knowledge/common_KBs/guideline/`
- 跨專案 ADR 路徑從 `knowledge/ADRs/` 移至 `knowledge/common_KBs/ADRs/`
- Step 1.5 自動載入清單：新增 `knowledge/common_KBs/tech-research/`（技術探討與研究筆記），說明文字同步更新
- Step 3 動態路徑注入：新增技術研究路徑變數
- BACKEND / QA Subagent prompts：必讀文件新增第 5 項「技術研究」，專案索引順延為第 6 項
- 回答規則知識庫限定：新增 `common_KBs/tech-research/` 為允許來源

---

## [1.0] — 初版

### Added
- 多角色 AI Agent：BACKEND / QA / SRE / PM / CONSULTANT / REVIEWER
- MULTI 模式：同一個 response 並行派發 BACKEND + QA 兩個 Subagent 分析同一 Story，完成後彙整輸出
- Knowledge Hub 整合：自動讀取 `$KB_ROOT`、共用規範、跨專案 ADR、各選定專案 MASTER_INDEX
- 多專案 KB 支援：掃描所有 `_KBs` 子資料夾，供使用者選擇載入範圍
- Jira MCP 整合：輸入 ticket 單號自動拉取 Story 內容，失敗則改請使用者貼文字
- 嚴格知識庫限定回答規則：禁止使用訓練資料或 KB 外知識，找不到資訊時明確告知
- 每則回答附引用來源區塊（📚 參考來源），多來源時逐一列出
