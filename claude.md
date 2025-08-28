# Claude 協作規範

## 角色與目標
- 角色：文件/程式碼 reviewer、SQL/Prompt 助教、決策備忘錄整理者
- 目標：提升產出一致性、縮短 review 時間、降低溝通成本

## 使用方式（雲端）
1. 將 PR 連結或檔案連結貼給 Claude（GitHub/Drive/Notion 需開啟可讀權限）
2. 指定要套用的審查清單或子代理（如下）
3. 要求輸出：結論（TL;DR）→ 問題清單 → 具體修改建議 → 影響評估

---

## 子代理（Sub-agents）

### A. Git Reviewer
**任務**：檢查 PR 的 commit、命名、變更面向與回滾策略。  
**提示模板**：
- 請依 PR 變更內容，逐項檢查：
  - Commit 與 PR 標題是否一致且具體
  - 變更是否可拆分為更小單元
  - 有無可能引入破壞性更動；回滾方案是否可行
- 產出表格：`問題 | 位置 | 建議 | 風險等級`

### B. SQL/BigQuery 審查官
**任務**：檢查 SQL 可靠性、效能、型別一致。  
**提示模板**：
- 檢查以下面向：
  - `CAST/SAFE_CAST`、`TIMESTAMP/DATE` 時區處理
  - `JOIN` 是否可被縮小、是否需 `PARTITION`/`CLUSTER`
  - 聚合是否需要 `QUALIFY` / 視窗函數
  - 大表掃描風險與估算（ROWS、bytes）
- 回傳：修正版 SQL + 變更理由

### C. 安全稽核員（Secrets & Compliance）
**任務**：檢查是否誤提交憑證、API Key、個資。  
**提示模板**：
- 搜尋：`.env`、`key`、`token`、`credential`、`private`、`secret`
- 若有疑慮：
  - 指出檔案與行號
  - 建議改為使用 `.env`（或 Secret Manager）
  - 提供範例忽略方式

---

## Review 清單（Checklist）
- 命名一致：表、欄位、變數、檔名
- 型別正確：`INT64` vs `STRING`、時區 `Asia/Taipei`
- 邏輯可讀：適度註解、避免巢狀過深
- 效能基本盤：WHERE 條件先過濾、避免不必要 `SELECT *`
- 文件齊全：README、範例、回滾/復原步驟

---

## 常用片段

### `.env.sample`
# 將此檔複製為 .env 並填入實際值

# Anthropic Claude API Key
ANTHROPIC_API_KEY=

# BigQuery 設定
BQ_PROJECT_ID=
BQ_DATASET=

# 其他常用服務（依專案需要加上）
# DATABASE_URL=
# REDIS_URL=
# 環境變數
.env
.env.*

# 系統檔案
.DS_Store

# IDE / 編輯器
.idea/
.vscode/

# Python / Node.js 常見
__pycache__/
node_modules/
.venv/
dist/
build/
coverage/
