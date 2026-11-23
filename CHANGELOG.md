# 更新日誌 (ChangeLog)

本專案遵循語意化版本（Semantic Versioning）規範記錄所有版本變更。

## [2.3.4] - 2026-11-23

### ⚙️ 推論引擎升級 (Engine Upgrade)
- **llama.cpp 核心引擎版本升級**：
  - 官方預編譯套件由 `b12436` 升級至最新發行版 `b12651`。
  - 快取與連線檢查目錄遷移至 `/content/llama-direct-b12651`，維持斷點續傳與已下載資產相容性。
  - 同步更新內建官方直連預編譯資產清單 (`_KNOWN_ASSETS`)，收錄 `b12651` 官方 CUDA 12.8 / 13.5 套件與對應 `cudart` 下載連結及精確檔案大小。

### 🤖 模型清單調整 (Model Catalog Update)
- **替換預設模型選項**：
  - 將 `Qwen3.8-27B Coder390 Q2LynnStyle` 替換為 `Inkling-2-Mini UD-Q3_K_XL`（來源：`unsloth/Inkling-2-Mini-GGUF`，主模型：`Inkling-2-Mini-UD-Q3_K_XL.gguf`，別名：`inkling-2-mini-q3kxl`）。
  - 內建多模態視覺投影檔設定：`mmproj-BF16.gguf`，支援自動下載與載入。

### 📝 文件與版本同步 (Documentation & Sync)
- 同步更新標準繁中版 (`colab-llamacpp-hub.ipynb`)、96G 繁中專用版 (`colab-llamacpp-hub_96G.ipynb`)、標準英文版 (`colab-llamacpp-hub_EN.ipynb`) 與 96G 英文專用版 (`colab-llamacpp-hub_96G_EN.ipynb`)。
- 於繁體中文與英文版 `README.md` / `README_EN.md` 頂部明確揭露目前內建 `llama.cpp` 版本 (`b12651`)。

---

## [2.3.3] - 2026-11-16

### ⚙️ 推論引擎升級 (Engine Upgrade)
- **llama.cpp 核心引擎版本升級**：
  - 官方預編譯套件由 `b12173` 全面升級至最新發行版 `b12436`。
  - 快取與連線檢查目錄全面遷移至 `/content/llama-direct-b12436`，維持斷點續傳與已下載資產相容性。
  - 同步更新內建官方直連預編譯資產清單 (`_KNOWN_ASSETS`)，收錄 `b12436` 官方 CUDA 12.8 / 13.5 套件與對應 `cudart` 下載連結及精確檔案大小，確保免 API 直連下載的高穩定性。
- **全系列 Notebook 與文件同步升級**：
  - 同步更新標準繁中版 (`colab-llamacpp-hub.ipynb`)、96G 繁中專用版 (`colab-llamacpp-hub_96G.ipynb`)、標準英文版 (`colab-llamacpp-hub_EN.ipynb`) 與 96G 英文專用版 (`colab-llamacpp-hub_96G_EN.ipynb`)。
  - 同步更新繁體中文與英文版 `README.md` / `README_EN.md`。

---

## [2.3.2] - 2026-11-09

### 🔒 遠端橋接固定版本鎖定 (SUMA Remote AI Bridge Fixed Version Pinning)
- **SUMA Remote AI Bridge 改用固定發行版下載**：
  - 移除啟動時透過 GitHub Releases API 及官網下載頁的動態版本偵測機制。
  - 直接鎖定採用穩定發行版本 `0.3.6.0 Beta` (`0360`)：`https://github.com/marchfun1/SUMA-Remote-AI-Bridge/releases/download/0360/SUMA-Remote-AI-Bridge_0.3.6.0_Beta-linux-{arch}.tar.gz`。
  - 內建精確檔案大小校驗（`8,190,363 bytes` / amd64），完全杜絕在 Google Colab 共用 IP 環境下因查詢外部 API 導致之 HTTP 403 限流或 HTTP 504 逾時風險。
- **全系列 Notebook 與文件同步升級**：
  - 同步更新標準繁中版 (`colab-llamacpp-hub.ipynb`)、96G 繁中專用版 (`colab-llamacpp-hub_96G.ipynb`)、標準英文版 (`colab-llamacpp-hub_EN.ipynb`) 與 96G 英文專用版 (`colab-llamacpp-hub_96G_EN.ipynb`)。
  - 同步更新繁體中文與英文版 `README.md` / `README_EN.md`。

---

## [2.3.1] - 2026-11-07

### ⚙️ 推論引擎升級與連線可靠度最佳化 (Engine Upgrade & Reliability Fixes)
- **llama.cpp 核心引擎版本升級**：
  - 官方預編譯套件由 `b12097` 全面升級至最新發行版 `b12173`。
  - 快取目錄全面遷移至 `/content/llama-direct-b12173`，維持斷點續傳與已下載資產相容性。
- **解決 GitHub API 限流與 504 逾時異常 (Fix API Gateway Timeout & Rate Limits)**：
  - **內建官方直連資產清單 (`_KNOWN_ASSETS`)**：為 `b12173` 內建完整官方 CUDA 預編譯套件（CUDA 12.8 / 13.5）與對應 `cudart` 下載連結，免去查詢 GitHub REST API 步驟，徹底根除 Google Colab 共用 IP 常見的 `HTTP 504: Gateway Time-out` 與 `HTTP 403: Rate Limit` 問題。
  - **動態直連下載備援機制 (`_generate_candidate_assets`)**：當查詢 GitHub API 失敗時，自動降級切換至官方直連資產命名規則下載，確保流程穩定不中斷。
  - **CUDA 12.x 次版本向下相容判定**：改進 `_candidates` 套件匹配邏輯，支援 CUDA 12.x 系列主版本相容測試，避免因 Colab 驅動版本微幅落後而誤判略過。
- **全系列 Notebook 與文件同步升級**：
  - 同步更新標準繁中版 (`colab-llamacpp-hub.ipynb`)、96G 繁中專用版 (`colab-llamacpp-hub_96G.ipynb`)、標準英文版 (`colab-llamacpp-hub_EN.ipynb`) 與 96G 英文專用版 (`colab-llamacpp-hub_96G_EN.ipynb`)。
  - 同步更新繁體中文與英文版 `README.md` / `README_EN.md`。

---

## [2.3.0] - 2026-11-05

### 🌐 雙語獨立架構拆分 (Dedicated Bilingual Architecture Split)
- **繁體中文與英文獨立版本解耦**：
  - 將標準版與 96G 專用版全面拆分獨立為純繁體中文版與純英文版 Notebook：
    - 中文版維持原檔名：`colab-llamacpp-hub.ipynb`、`colab-llamacpp-hub_96G.ipynb`。
    - 英文版採用 `_EN` 語言後綴：`colab-llamacpp-hub_EN.ipynb`、`colab-llamacpp-hub_96G_EN.ipynb`。
  - 頂部規格說明儲存格回歸為純 Markdown 格式，使用者匯入 Colab 後即可直觀檢視完整引導與模型規格，省去手動執行儲存格的步驟。
  - 移除所有執行階段動態語言轉換機制（`_msg` 函式與 `LANGUAGE` 下拉參數），日誌反饋、表單介面（`# @title` / `# @markdown`）、保活監控與錯誤訊息全面依目標語言純粹化。
- **專案規範與文件同步**：
  - 更新 `README_EN.md` 快速開始指引，對應全新英文版 Notebook 檔名。
  - 於專案核心技能規則中新增雙語同步維護準則（Bilingual Synchronization），確保後續迭代雙語版本一致。

### 🔄 SUMA Remote AI Bridge 自動取得最新版機制 (Auto-Fetch Latest SUMA Release)
- **三重動態版本偵測與下載機制 (`_start_suma`)**：
  - **第一重（GitHub Releases API）**：啟動時自動查詢 `https://api.github.com/repos/marchfun1/SUMA-Remote-AI-Bridge/releases/latest`，動態取得最新發行版標籤（如 `0360`）、檔案大小與對應系統架構（`-linux-amd64.tar.gz` / `-linux-arm64.tar.gz`）之下載連結。
  - **第二重（官網下載區解析備援）**：當 Colab 共用 IP 遭遇 GitHub API 匿名請求限流（HTTP 403 Rate Limit）時，自動改連官網產品頁 `https://suma.tw/product/ai-bridge/` 解析下載區最新的 Linux CLI 套件連結與版本號，徹底擺脫 API 額度限制。
  - **第三重（穩定版保底預設值）**：若遇極端網路環境導致前兩重查詢皆無法連線，自動降級使用 `0360`（`v0.3.6.0 Beta`）預設連結，確保遠端算力橋接服務穩定啟動。

---

## [2.2.2] - 2026-11-04

### ⚙️ 推論引擎升級 (Engine Upgrade)
- **llama.cpp 核心引擎版本升級**：
  - 官方預編譯套件由 `b12032` 升級至最新發行版 `b12097`。
  - 安裝與模型快取目錄遷移至 `/content/llama-direct-b12097`，保持斷點續傳與已下載資產相容性。
  - 同步更新標準版 (`colab-llamacpp-hub.ipynb`) 與 96G 專用版 (`colab-llamacpp-hub_96G.ipynb`) 之二進位檔管理及連線檢查邏輯。

---

## [2.2.1] - 2026-11-03

### 🔄 連線保活與中斷重啟機制 (Keep-Alive & Reconnection Recovery)
- **核心常駐保活監控循環 (`_keep_alive_watchdog`)**：
  - 移除受限於 iframe 沙盒隔離而易失效之前端腳本，改由 Python 核心於模型服務啟動完成後直接進入常駐執行狀態。
  - 儲存格持續維持執行（Running）以徹底防止 Google Colab 觸發「無操作閒置逾時（Idle Timeout）」斷線機制與釋放 GPU。
  - 保活心跳頻率設定為 **每 5 分鐘（300 秒）** 一次，定期回報推論端點健康狀態與已穩定運行時數，降低日誌干擾。
- **支援熱切換與斷線重連保活**：
  - 模型切換（Switch Model）與重連恢復（Reconnect）單元同步整合常駐保活機制，維持不中斷連線。
- **斷線重連與檔案完整性健康檢查單元 (`Service Health Check & Reconnect`)**：
  - 於 Notebook 最下方新增專屬重連檢查區塊。
  - **虛擬機重置/模型清空防呆檢測**：自動檢查磁碟內 GGUF 模型與套件完整性；若偵測到執行階段已遭 Google 回收清空，主動發出警告並指引使用者由第一格重新執行初始化。
  - **服務存活狀態檢測與自動拉起**：檢查 `llama.cpp` 模型推論端點與 `SUMA AI Bridge` 守護程序；若檔案尚存但服務中斷，自動拉起推論服務與連線橋接，省去手動重啟流程。

---

## [2.2.0] - 2026-11-02

### 🌐 國際化與動態雙語切換 (Internationalization & Dynamic Language Switching)
- **頂部獨立語言切換表單 (`language-and-instructions`)**：
  - 首個單元改為獨立 Colab 表單檢視，提供 `LANGUAGE`（繁體中文／English）下拉選單。
  - 採用 `IPython.display.display(Markdown(...))` 動態渲染完整規格說明與注意事項，支援單元執行後即時切換全篇說明語言。
- **全流程雙語狀態反饋與防呆訊息**：
  - 核心執行單元（Cell 1）與模型熱切換單元（Cell 3）全面串接 `_msg(zh, en)` 函式，自動依所選語言即時顯示日誌、下載進度與連線狀態。
  - 修正進度列（`_Progress`）在英文模式下字串格式化異常問題，使計時與下載統計輸出清晰正確。

### 🔍 模型規格與多模態支援精準校正 (Model Specifications Correction)
- **Gemma 4 12B 原生架構上下文校正**：
  - 依官方模型卡更新原生上下文規格為最高 256K tokens。
- **多模態（MMProj）全系列支援確認**：
  - 確認 Qwen3.5-9B 與 Qwen3.8-27B 官方/社群倉庫均內建提供相容之 `mmproj-BF16.gguf` / `mmproj-F16.gguf`，更新說明並確認勾選後可自動匹配下載載入。

---

## [2.1.0] - 2026-11-02

### 🚀 模型更新與多模態增強 (Model Updates & Multimodal Enhancement)
- **換入高效能程式設計推理模型**：
  - 將原本的 `Qwen3.8-27B UD-IQ3_XXS` 替換為社群精調高評分之 `Qwen3.8-27B Coder390 Q2LynnStyle`（檔案名稱：`Qwen3.8-27B-Coder390-EfficientThink-Q2LynnStyle-Q8MTP.gguf`）。
  - 模型來源為 `nerkyor/Qwen3.8-27B-Coder390-EfficientThink-Opus5.5-GPT6Astra-Grok4.7-DSV4Pro-K3-SFT-RLOO-GGUF-NInfer`，內建 MTP（多 Token 預測）頭與 EfficientThink 最佳化，有效消除推理死循環與尾部重複輸出問題。
  - 整合專屬多模態投射器 `GGUF/mmproj-Qwen3.8-27B-Q8_0.gguf`，兼備程式碼理解與視覺圖文多模態推論能力。
  - 設定預設別名為 `qwen3.8-27b-coder390`。

### ⚙️ 推論引擎升級與相容性修復 (Engine Upgrade & Bug Fixes)
- **llama.cpp 核心引擎版本升級**：
  - 官方預編譯套件由 `b12020` 升級至最新發行版 `b12032`。
  - 預設安裝與模型本機快取目錄同步遷移至 `/content/llama-direct-b12032`，保留既有下載進度與斷點續傳。
- **修正 llama-server 命令列參數相容性**：
  - 修正啟動指令中對 `--models-path` 的檢查與追加邏輯，僅在執行檔明確支援時傳入，徹底修復執行檔參數檢查時因缺少 `--models-path` 導致的 `RuntimeError: 執行檔不支援參數：['--models-path']` 例外。

### 🔀 Web UI 多模型清單 (Router Mode)
- **改用 llama-server 路由模式啟動**：
  - 以 `--models-preset` 自動產生 `models-preset.ini`，將本機所有已下載模型（含各自 MMProj 與最佳化參數）列入 Web UI 模型清單，切換新模型後先前的模型仍可直接選取。
  - 設定 `--models-max 1`，同時僅保留一個模型於 GPU，切換時自動卸載前一模型，避免 T4 顯卡記憶體不足（OOM）。
  - 啟動後自動透過 `/models/load` 載入表單所選模型並等待就緒；若執行檔不支援路由模式，自動退回單模型模式。

### 📐 上下文長度最佳化 (Context Window Options)
- **上下文選單調整**：
  - 下拉選項調整為 `64K`、`128K`、`256K` 三段配置，精準適配 Colab T4 15 GB GPU 算力情境與長文本分析需求。

### 📝 文件與介面識別 (Documentation & UI Standardization)
- **版本資訊標準化**：
  - 於 Notebook 頂部文字說明儲存格、Colab 應用程式詮釋資料（Metadata）以及中英文 README 標題最前方統一標註 `v2.1.0` 版本號。

---

## [2.0.0] - 2026-11-01

### 🎉 主要架構建立與多模型熱切換發布
- **SUMA Remote AI Bridge 算力串接**：
  - 深度整合 SUMA Remote AI Bridge 技術，以 `--port 8081` 直接將 Google Colab 遠端 GPU 算力安全對接至本機客戶端，無須公網 IP 或複雜反向代理。
- **免編譯即時啟動**：
  - 自動下載並解壓官方預編譯套件（`b12020`），免去耗時的本機原始碼編譯流程。
- **雙表單熱切換設計與 SUMA 連線維持**：
  - 提供「初次啟動」與「後續切換」表單。切換模型時僅重啟 `llama-server`（`stop_model_service()`），保持 SUMA AI Bridge 持續運行，切換後無需重新配對。
- **解決 TCP 通訊埠衝突（Port 8081）**：
  - 在服務切換與啟動檢驗時加入 `SO_REUSEADDR` 與重試等待機制，避免切換模型時因通訊埠未及時釋放（TIME_WAIT）導致的 `Address already in use` 錯誤。
- **llama.cpp Web UI 多模型原生切換**：
  - 建立 `active_models` 目錄並啟動 `--models-path`，讓本機所有已下載的新舊模型皆完整列於 llama.cpp Web UI 的模型切換按鈕中，使用者可直接在網頁端隨時切換。
- **模型陣容調整與排序**：
  - 移除 `Qwen3.8-27B UD-Q3_K_XL`，新增 `Qwen3.8-27B UD-Q2_K_XL` 並排列於 `Qwen3.5-9B Q5_K_M` 之後。
  - 完整模型陣容：Gemma 4 12B QAT Q4_0、Qwen3.5-9B Q5_K_M、Qwen3.8-27B UD-Q2_K_XL、Qwen3.8-27B UD-IQ3_XXS。
- **智慧容錯與斷點續傳**：
  - 當使用者選取純語言模型但開啟 MMProj 時，自動發出提示並智慧降級為純文字推論模式，避免執行中斷。
  - 內建已下載模型狀態檢視函式 `list_downloaded_models()`，便於掌握目前本機快取與運行狀況。
