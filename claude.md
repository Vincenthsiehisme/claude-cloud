# Claude Cloud - 專案協作規範

## 專案概述
**專案名稱**：claude-cloud  
**技術堆疊**：[請補充：如 React, Node.js, BigQuery 等]  
**主要目標**：提供雲端 AI 協作平台，提升開發效率與程式碼品質  

## Claude Code 角色定義
- **主要角色**：文件/程式碼 reviewer、SQL/Prompt 助教、決策備忘錄整理者
- **核心目標**：提升產出一致性、縮短 review 時間、降低溝通成本
- **工作模式**：結構化審查 → 具體建議 → 風險評估

## 專案結構
```
claude-cloud/
├── src/                 # 主要程式碼
├── docs/               # 專案文件
├── tests/              # 測試檔案
├── config/             # 配置檔案
├── .env.sample         # 環境變數範本
└── .github/            # GitHub workflows
```

## 開發流程
1. **準備階段**：複製 `.env.sample` 為 `.env` 並設定必要變數
2. **開發階段**：遵循程式碼規範，撰寫對應測試
3. **審查階段**：使用子代理進行多面向檢查
4. **部署階段**：確保安全性檢查通過

---

## 子代理系統（Sub-agents）

### A. Git Reviewer 🔍
**專責任務**：檢查 PR 的 commit 品質、命名規範、變更策略
**審查要點**：
- Commit message 遵循 Conventional Commits
- PR 標題清楚描述變更內容
- 變更範圍適當，可獨立測試
- 具備明確的回滾策略

**輸出格式**：
| 問題分類 | 檔案位置 | 具體建議 | 風險等級 | 優先度 |
|---------|---------|---------|---------|--------|
| 命名 | src/utils.js:15 | 函數名應更具描述性 | Low | P2 |

### B. SQL/BigQuery 審查官 📊
**專責任務**：確保 SQL 查詢的效能、正確性與最佳實務
**檢查面向**：
- 資料型別正確性（`CAST`/`SAFE_CAST` 使用）
- 時區處理（統一使用 `Asia/Taipei`）
- JOIN 最佳化與分區策略
- 大表掃描風險評估
- 視窗函數與聚合函數適當使用

**輸出內容**：修正後的 SQL + 效能改善說明

### C. 安全稽核員 🛡️
**專責任務**：檢測敏感資訊洩露風險，確保合規性
**掃描目標**：
- API Keys、Token、憑證
- 個人資訊與敏感資料
- 配置檔案安全性
- 權限設定合理性

**行動方案**：立即移除 + 環境變數化 + `.gitignore` 更新

### D. 程式碼品質檢查員 ⚡
**專責任務**：程式碼結構、效能、可維護性審查
**評估標準**：
- 程式碼可讀性與註解完整度
- 函數複雜度與重複性
- 錯誤處理機制
- 測試涵蓋率

---

## 品質檢查清單

### 🎯 命名規範
- [ ] 檔案名使用 kebab-case
- [ ] 變數/函數使用 camelCase  
- [ ] 常數使用 UPPER_SNAKE_CASE
- [ ] 資料表/欄位命名一致性

### 🔒 安全檢查
- [ ] 無硬編碼憑證或 API Keys
- [ ] 環境變數正確使用
- [ ] 輸入資料驗證完整
- [ ] 權限控制適當

### 📈 效能最佳化
- [ ] SQL 查詢使用適當索引
- [ ] 避免 N+1 查詢問題
- [ ] 前端資源最佳化
- [ ] API 回應時間合理

### 📚 文件完整性
- [ ] README 包含安裝與使用說明
- [ ] API 文件更新
- [ ] 變更日誌記錄
- [ ] 回滾步驟文件

---

## 環境設定範本

### 必要環境變數
```bash
# === 核心服務配置 ===
NODE_ENV=development
PORT=3000
APP_NAME=claude-cloud

# === AI 服務 ===
ANTHROPIC_API_KEY=your-claude-api-key-here
OPENAI_API_KEY=your-openai-api-key-here

# === 資料庫配置 ===
BQ_PROJECT_ID=your-project-id
BQ_DATASET=your-dataset
DATABASE_URL=your-database-connection-string

# === 雲端服務 ===
AWS_REGION=ap-southeast-1
GCP_PROJECT_ID=your-gcp-project

# === 安全設定 ===
JWT_SECRET=your-jwt-secret-key
CORS_ORIGINS=http://localhost:3000,https://yourdomain.com

# === 監控與日誌 ===
LOG_LEVEL=info
SENTRY_DSN=your-sentry-dsn
```

## 開發規範
- **程式碼風格**：使用 Prettier + ESLint
- **提交規範**：Conventional Commits
- **分支策略**：Git Flow
- **測試要求**：單元測試 > 80% 覆蓋率

## 部署流程
1. 通過所有自動化測試
2. 安全掃描無高風險項目
3. 效能測試達標
4. 文件同步更新

---

## Claude Code 使用指南
### 啟動專案
```bash
# 安裝依賴
npm install

# 設定環境變數
cp .env.sample .env
# 編輯 .env 填入實際值

# 啟動開發環境
npm run dev
```

### 常用指令
- `npm test` - 執行測試套件
- `npm run lint` - 程式碼風格檢查
- `npm run build` - 建構生產版本
- `npm run deploy` - 部署至雲端

### 與 Claude 協作
1. 上傳 PR 連結或檔案
2. 指定使用的子代理類型
3. 要求結構化輸出：TL;DR → 問題清單 → 修改建議 → 影響評估

## 緊急應變
- **回滾程序**：詳見 `docs/rollback-procedures.md`
- **事故應對**：依循 `docs/incident-response.md`
- **聯絡資訊**：
