# 更新日誌 (ChangeLog)

本專案遵循語意化版本（Semantic Versioning）規範記錄所有版本變更。

## [2.2.1] - 2026-11-03

### 🔄 連線保活與中斷重啟機制 (Keep-Alive & Reconnection Recovery)
- **前端防閒置保活機制 (`Keep-Alive`)**：
  - 於啟動儲存格中注入定時保活 JavaScript，每 60 秒自動模擬點擊 Colab 連線工具列，持續向後端傳遞活躍訊號，避免因長時間無網頁互動而被 Google Colab 判定閒置進而中斷連線與回收 GPU。
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
  - 設定 `--models-max 1`，同時僅保留一個模型於 GPU，切換時自動卸載前一模型，避免 T4 顯存不足（OOM）。
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
