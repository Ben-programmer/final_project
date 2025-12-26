# Dify RAG Chat Bot - 期末專案

## 專案簡介

這是一個基於 Dify 平台建構的 RAG（Retrieval-Augmented Generation）聊天機器人專案。Dify 是一個開源的 LLM 應用程式開發平台，提供了視覺化的工作流程編排、多模型支援、RAG 管線等功能。

## 專案結構

```
dify_rag_chat_bot/
├── dify/                          # Dify 核心程式碼
│   ├── api/                       # Flask 後端 API
│   │   ├── app.py                # 主應用程式入口
│   │   ├── controllers/          # API 控制器
│   │   ├── core/                 # 核心功能模組
│   │   ├── models/               # 資料庫模型
│   │   ├── services/             # 業務邏輯服務
│   │   └── requirements.txt      # Python 依賴套件
│   ├── web/                      # Next.js 前端應用
│   │   ├── app/                  # 應用程式頁面
│   │   ├── components/           # React 元件
│   │   ├── service/              # API 服務層
│   │   └── package.json          # Node.js 依賴套件
│   ├── docker/                   # Docker 配置檔案
│   └── docs/                     # 多語言文件
├── a_lvr_land_c.csv              # 土地資料集
├── data.csv                      # 資料集 1
├── data-1.csv                    # 資料集 2
└── opendata_rent_0811_1021_v1.csv # 租屋公開資料集

```

## 資料集說明

本專案包含以下資料集：

1. **a_lvr_land_c.csv** - 土地實價登錄資料
2. **data.csv** - 主要資料集
3. **data-1.csv** - 輔助資料集
4. **opendata_rent_0811_1021_v1.csv** - 租屋資訊公開資料（2022年8月11日至10月21日）

這些資料集用於訓練和增強 RAG 聊天機器人的知識庫，特別是針對台灣不動產和租屋市場的查詢。

## 主要功能

- **RAG 問答系統**：結合檢索與生成技術，提供精確的答案
- **多資料來源整合**：支援 CSV、PDF、TXT 等多種格式的知識庫
- **視覺化工作流程**：透過 Dify 平台編排複雜的 AI 應用流程
- **多模型支援**：可整合 OpenAI、Anthropic、本地模型等
- **即時對話**：提供流暢的聊天介面和 SSE 串流回應

## 技術棧

### 後端
- **Python 3.13** - 核心程式語言
- **Flask 3.1** - Web 框架
- **SQLAlchemy** - ORM
- **Celery** - 非同步任務處理
- **Redis** - 快取和訊息佇列
- **PostgreSQL** - 主要資料庫

### 前端
- **Next.js** - React 框架
- **TypeScript** - 型別安全
- **Tailwind CSS** - UI 樣式

### AI/ML
- **LangChain** - LLM 應用框架
- **ChromaDB** - 向量資料庫
- **NLTK** - 自然語言處理
- **OpenAI/Anthropic APIs** - LLM 服務

## 安裝與設定

### 前置需求

- Python 3.10 或以上版本
- Node.js 18 或以上版本
- PostgreSQL 15 或以上版本
- Redis 7 或以上版本
- Docker（選用）

### 使用 Docker 安裝（推薦）

1. 複製專案：
```bash
git clone https://github.com/Ben-programmer/final_project.git
cd final_project/dify/docker
```

2. 複製環境變數檔案：
```bash
cp middleware.env.example middleware.env
```

3. 啟動服務：
```bash
docker compose up -d
```

4. 訪問應用程式：
- Web UI: http://localhost/
- API: http://localhost/api

### 手動安裝

#### 後端設定

1. 進入 API 目錄：
```bash
cd dify/api
```

2. 建立虛擬環境並安裝依賴：
```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. 設定環境變數：
```bash
cp .env.example .env
# 編輯 .env 檔案，填入必要的配置
```

4. 初始化資料庫：
```bash
flask db upgrade
```

5. 啟動後端服務：
```bash
flask run --host=0.0.0.0 --port=5001
```

#### 前端設定

1. 進入 Web 目錄：
```bash
cd dify/web
```

2. 安裝依賴：
```bash
npm install
# 或使用 pnpm
pnpm install
```

3. 設定環境變數：
```bash
cp .env.example .env.local
# 編輯 .env.local，設定 API 端點
```

4. 啟動開發伺服器：
```bash
npm run dev
```

5. 訪問前端：http://localhost:3000

## 使用說明

### 1. 建立應用程式

1. 登入 Dify 平台
2. 點擊「建立應用程式」
3. 選擇「聊天助手」類型
4. 配置應用程式基本資訊

### 2. 上傳知識庫

1. 進入「知識庫」頁面
2. 點擊「新增知識庫」
3. 上傳 CSV 檔案（如 `opendata_rent_0811_1021_v1.csv`）
4. 選擇適當的分段策略和向量化模型
5. 等待知識庫處理完成

### 3. 配置 RAG

1. 在應用程式設定中啟用「知識檢索」
2. 選擇剛才建立的知識庫
3. 調整檢索參數（Top K、Score Threshold 等）
4. 設定提示詞模板

### 4. 測試對話

在對話介面輸入問題，例如：
- 「台北市的平均租金是多少？」
- 「有哪些三房兩廳的租屋資訊？」
- 「新北市板橋區的租金行情如何？」

## 資料庫遷移

執行資料庫遷移：

```bash
cd dify/api
flask db upgrade
```

如果遇到導入錯誤，確保已安裝所有依賴：

```bash
pip install -r requirements.txt
```

## 開發指南

### API 開發

後端 API 位於 `dify/api/controllers/`，按功能模組組織：

- `console/` - 管理後台 API
- `web/` - 前端應用 API
- `service_api/` - 第三方服務 API

### 前端開發

前端採用 Next.js App Router 架構：

- `app/` - 頁面路由
- `components/` - 可重用元件
- `service/` - API 呼叫服務
- `context/` - React Context 狀態管理

### 測試

執行測試：

```bash
# 後端測試
cd dify/api
pytest

# 前端測試
cd dify/web
npm run test
```

## 環境變數說明

### 後端環境變數

```env
# 基本配置
SECRET_KEY=your-secret-key
FLASK_ENV=development

# 資料庫
DB_USERNAME=postgres
DB_PASSWORD=your-password
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=dify

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# 向量資料庫
VECTOR_STORE=chroma
CHROMA_HOST=localhost
CHROMA_PORT=8000

# LLM API
OPENAI_API_KEY=your-openai-key
ANTHROPIC_API_KEY=your-anthropic-key
```

### 前端環境變數

```env
NEXT_PUBLIC_API_PREFIX=http://localhost:5001
NEXT_PUBLIC_PUBLIC_API_PREFIX=http://localhost:5001
```

## 故障排除

### 常見問題

1. **ImportError: No module named 'redis'**
   ```bash
   pip install redis
   ```

2. **資料庫連接失敗**
   - 確認 PostgreSQL 服務已啟動
   - 檢查資料庫連接字串是否正確

3. **前端無法連接後端**
   - 確認後端服務已啟動
   - 檢查 CORS 設定
   - 驗證 API 端點配置

4. **向量資料庫錯誤**
   - 確認 ChromaDB 服務已啟動
   - 檢查向量化模型是否已下載

## 貢獻指南

歡迎提交 Issue 和 Pull Request！

1. Fork 本專案
2. 建立功能分支：`git checkout -b feature/your-feature`
3. 提交變更：`git commit -m 'Add some feature'`
4. 推送到分支：`git push origin feature/your-feature`
5. 提交 Pull Request

## 授權

本專案基於 Dify 開源專案，遵循其原有授權條款。詳見 [LICENSE](./dify/LICENSE) 檔案。

Dify 使用 Apache 2.0 License with additional terms。

## 相關連結

- [Dify 官方網站](https://dify.ai)
- [Dify 文件](https://docs.dify.ai)
- [Dify GitHub](https://github.com/langgenius/dify)
- [Dify Discord 社群](https://discord.gg/FngNHpbcY7)

## 聯絡方式

如有任何問題或建議，歡迎聯繫：

- GitHub Issues: [提交問題](https://github.com/Ben-programmer/final_project/issues)
- Email: [你的信箱]

## 致謝

感謝 Dify 團隊提供如此優秀的開源平台，以及所有貢獻者的努力。

---

**最後更新日期**：2025年12月26日
