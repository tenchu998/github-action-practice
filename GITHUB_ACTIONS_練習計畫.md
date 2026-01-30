# GitHub Actions 練習計畫 📚

> 目標：從零開始學習 GitHub Actions，建立 CI/CD 流程

---

## 📋 練習待辦事項清單

### 階段一：環境準備 ✅
- [ ] **1.1** 確認 Git 設定使用個人帳號
  ```bash
  # 檢查目前設定
  git config user.name
  git config user.email
  
  # 設定為個人帳號（如果需要）
  git config user.name "你的GitHub用戶名"
  git config user.email "你的個人email"
  ```

- [ ] **1.2** 在 GitHub 上建立新的 Repository
  1. 登入你的個人 GitHub 帳號
  2. 點擊右上角 `+` → `New repository`
  3. 名稱：`github-action-practice`
  4. 設為 Public（免費帳號 Actions 分鐘數較多）
  5. 不要勾選 Initialize（因為本地已有檔案）

- [ ] **1.3** 連接本地專案到 GitHub
  ```bash
  git remote add origin https://github.com/你的用戶名/github-action-practice.git
  git branch -M main
  git push -u origin main
  ```

---

### 階段二：第一個 GitHub Action（Hello World）🎯
- [ ] **2.1** 建立 workflow 目錄結構
  ```bash
  mkdir -p .github/workflows
  ```

- [ ] **2.2** 建立第一個 workflow 檔案
  - 檔案位置：`.github/workflows/hello.yml`
  - 內容見下方範例

- [ ] **2.3** Push 並觀察 Action 執行
  ```bash
  git add .
  git commit -m "feat: add first github action"
  git push
  ```

- [ ] **2.4** 到 GitHub Repository → Actions 頁籤查看執行結果

---

### 階段三：建立 CI 流程（自動建置）🔨
- [ ] **3.1** 建立 CI workflow
  - 檔案：`.github/workflows/ci.yml`
  - 目標：自動執行 `npm install` 和 `npm run build`

- [ ] **3.2** 新增測試腳本
  - 安裝 Jest 或 Vitest
  - 寫一個簡單的測試
  - CI 中加入測試步驟

- [ ] **3.3** 設定 PR 觸發
  - 讓 CI 在 Pull Request 時自動執行

---

### 階段四：進階功能練習 🚀
- [ ] **4.1** 練習不同觸發條件
  - `push` - 推送時觸發
  - `pull_request` - PR 時觸發
  - `schedule` - 定時觸發（cron）
  - `workflow_dispatch` - 手動觸發

- [ ] **4.2** 使用 Matrix 策略
  - 在多個 Node.js 版本上測試

- [ ] **4.3** 使用 Cache
  - 快取 node_modules 加速建置

- [ ] **4.4** 使用 Secrets
  - 在 GitHub 設定 Secrets
  - 在 workflow 中使用

- [ ] **4.5** 練習 Job 之間的依賴
  - `needs` 關鍵字使用

---

### 階段五：實際應用場景 🌟
- [ ] **5.1** 自動部署到 GitHub Pages
- [ ] **5.2** 自動建立 Release
- [ ] **5.3** 自動發送通知（Slack/Discord）
- [ ] **5.4** Code Review 自動化（Linting）

---

## 📝 範例 Workflow 檔案

### 範例 1：Hello World（階段二用）
```yaml
# .github/workflows/hello.yml
name: Hello World

on:
  push:
    branches: [ main ]
  workflow_dispatch:  # 允許手動觸發

jobs:
  say-hello:
    runs-on: ubuntu-latest
    
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions! 🎉"
      
      - name: Show Date
        run: date
      
      - name: Show System Info
        run: |
          echo "OS: $RUNNER_OS"
          echo "Node version: $(node --version)"
```

### 範例 2：CI 建置（階段三用）
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
```

### 範例 3：Matrix 測試（階段四用）
```yaml
# .github/workflows/matrix.yml
name: Matrix Test

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      
      - run: npm ci
      - run: npm run build
```

---

## 🔗 實用資源

1. **官方文件**：https://docs.github.com/en/actions
2. **Workflow 語法**：https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions
3. **官方 Actions 市集**：https://github.com/marketplace?type=actions
4. **常用 Actions**：
   - `actions/checkout` - 拉取程式碼
   - `actions/setup-node` - 設定 Node.js
   - `actions/cache` - 快取
   - `actions/upload-artifact` - 上傳產出物

---

## ⏰ 建議學習時程

| 階段 | 預估時間 | 重點 |
|------|---------|------|
| 階段一 | 15-30 分鐘 | 環境設定 |
| 階段二 | 30 分鐘 | 理解基本概念 |
| 階段三 | 1 小時 | CI 實作 |
| 階段四 | 2-3 小時 | 進階功能 |
| 階段五 | 依需求 | 實際應用 |

---

## 💡 小提醒

1. 每次修改 workflow 檔案後都要 push 才會生效
2. 可以在 Actions 頁面看到詳細的執行日誌
3. workflow 檔案的縮排很重要，使用空格不要用 tab
4. 免費帳號每月有 2000 分鐘的 Actions 執行時間

---

**開始日期**：2026/01/30  
**狀態**：🚀 準備開始！
