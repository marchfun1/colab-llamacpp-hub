# v2.3.4 SUMA 遠端模型啟動器 (Colab llama.cpp Launcher)

[English](README_EN.md) | [繁體中文](README.md)

> **目前內建引擎版本**：`llama.cpp b12651` ｜ **SUMA Remote AI Bridge**：`0.3.6.0 Beta`

專為 Google Colab 設計的高效模型啟動與切換工具，基於 `llama.cpp` 官方預編譯二進位檔構建，支援 GGUF 格式、多模態（MMProj）、動態上下文長度切換，並**透過 [SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) 技術無縫存取遠端 Colab GPU 算力**。

---

## ⚡ 直接在 Colab 中開啟

| 版本 | 適用規格 | 檔案名稱 | 啟動連結 |
| :--- | :--- | :--- | :---: |
| **繁體中文版（標準）** | T4 GPU (15 GB VRAM / 免費版) | `colab-llamacpp-hub.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nec-U8HF8nX17F5zZrImr5xj5VeFY11v?usp=sharing) |
| **English（標準）** | T4 GPU (15 GB VRAM / Free Tier) | `colab-llamacpp-hub_EN.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nkrf9a1eVHEzES_QU5y00vf365FPwgnK?usp=sharing) |

---

## 🌟 核心特色

- **[SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) 算力串接**：採用 [SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) 技術，直接將遠端 Google Colab 的 GPU 算力安全對接到本地應用與客戶端，無需設定公網 IP、連接埠轉發（Port Forwarding）或繁瑣的反向代理。
- **零建置快速啟動**：直接下載並解壓 `llama.cpp` (b12651) 官方預編譯套件，省去漫長的本機編譯時間。
- **直觀表單介面**：利用 Colab Form 表單檢視，提供下拉式選單操作，預設折疊程式碼，保持視覺乾淨簡潔。
- **無縫熱切換**：提供「初次啟動」與「後續切換」表單，切換模型時自動平順終止既有推論程序並釋放 VRAM，無需重設 VM 執行階段。
- **多模態（MMProj）智慧容錯**：支援視覺多模態投射器載入；若選取模型不支援多模態，會自動降級為純文字推論，避免中斷報錯。
- **支援斷點續傳**：內建分塊下載與續傳機制，大幅節省更換或重複載入模型的時間與頻寬。

---

## 📋 支援模型與規格

預設內建多組經最佳化的開源 GGUF 模型：

| 模型名稱 | Hugging Face 來源 | 預設別名 | 多模態支援 (MMProj) |
| :--- | :--- | :--- | :---: |
| **Gemma 4 12B QAT Q4_0** | `google/gemma-4-12B-it-qat-q4_0-gguf` | `gemma-4-12b` | ✅ 支援 |
| **Qwen3.5-9B Q5_K_M** | `unsloth/Qwen3.5-9B-GGUF` | `qwen3.5-9b` | ✅ 支援 |
| **Qwen3.8-27B UD-Q2_K_XL** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-q2kxl` | ✅ 支援 |
| **Inkling-2-Mini UD-Q3_K_XL** | `unsloth/Inkling-2-Mini-GGUF` | `inkling-2-mini-q3kxl` | ✅ 支援 |

### 模型特色介紹

1. **Gemma 4 12B QAT Q4_0**
   - **模型特色**：Google 最新一代輕量高效能指令微調模型，經量化感知訓練（QAT）處理，兼顧低顯卡記憶體佔用與高精確度；支援多模態視覺投影（MMProj）。
   - **原生架構上下文**：最高 256K tokens（本啟動器支援最高配置至 256K）。

2. **Qwen3.5-9B Q5_K_M**
   - **模型特色**：阿里通義千問優秀的輕量多語言模型，採用 Q5_K_M 中高精度量化，推論流暢、中文理解與綜合推理表現均衡；亦支援搭配 MMProj 進行視覺理解。
   - **原生架構上下文**：最高 262,144 tokens（256K，本啟動器支援最高配置至 256K）。

3. **Qwen3.8-27B UD-Q2_K_XL**
   - **模型特色**：27B 中大參數量模型，透過極致的 UD-Q2 超低位元量化技術，讓 16GB 顯卡記憶體（如 Colab T4）也能載入大參數量模型進行高難度推理；亦支援多模態視覺投影。
   - **原生架構上下文**：最高 262,144 tokens（256K，本啟動器支援最高配置至 256K）。

4. **Inkling-2-Mini UD-Q3_K_XL**
   - **模型特色**：Thinking Machines 的 MoE 多模態模型，採用 Unsloth Dynamic 3.0 UD-Q3_K_XL 量化，兼顧低顯卡記憶體佔用、綜合推理與視覺理解能力。
   - **原生架構上下文**：最高 256K tokens（本啟動器支援最高配置至 256K）。

* 上下文長度（Context）可選：`64K`、`128K`、`256K`（請依 Colab 實際分配到的 GPU 記憶體配置，避免 OOM）。

### 96G 專用版內建模型與規格

專為 **96GB 高顯卡記憶體**（如 Colab 高階 GPU）環境設計，支援多檔案分片自動展開下載與斷點續傳，預設載入 **256K** 上下文長度：

| 版本 | 適用規格 | 檔案名稱 | 啟動連結 |
| :--- | :--- | :--- | :---: |
| **繁體中文版（96G）** | 高記憶體 GPU (96 GB VRAM) | `colab-llamacpp-hub_96G.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1PlOqL4JMTxaWYlyU2m4rdDwlmoE0yv5C?usp=sharing) |
| **English（96G）** | 高記憶體 GPU (96 GB VRAM) | `colab-llamacpp-hub_96G_EN.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/10Em-w1Y7xRSf7N56UBOc4_aENPjjoQJU?usp=sharing) |

| 模型名稱 | Hugging Face 來源 | 預設別名 | 多模態支援 (MMProj) |
| :--- | :--- | :--- | :---: |
| **Qwen3.8-Flash-Next UD-IQ4_XS** | `unsloth/Qwen3.8-Flash-Next-GGUF` | `qwen3.8-flash-next` | ✅ 支援 |

- **Qwen3.8-Flash-Next UD-IQ4_XS**
  - **模型特色**：高容量旗艦級推理模型，採用 Unsloth Dynamic UD-IQ4_XS 量化（共 3 個分片，總計約 87.25 GiB），充分發揮 96GB 顯卡記憶體優勢，提供極致長文本與複雜推理表現；亦支援搭配 `mmproj-BF16.gguf` / `mmproj-F16.gguf` 進行多模態視覺理解。
  - **原生架構上下文**：原生最高 262,144 tokens（256K），可擴展至最高 1,000,000 tokens（1M）；96G 專用版預設配置為 256K，支援 `64K` / `128K` / `256K` / `512K` / `1M` 選項。

---

## 🚀 快速開始

1. **開啟 Notebook**：將 `colab-llamacpp-hub.ipynb` 上傳或匯入至 Google Colab，或直接點擊上方「直接在 Colab 中開啟」表格中的啟動連結開啟。
2. **啟用 GPU 加速**：在 Colab 頂部選單點選 **執行階段** > **變更執行階段類型**，硬體加速器選擇 **T4 GPU** 或其他可用 GPU（顯卡記憶體至少 15GB 以上）。
3. **初次啟動**：
   - 找到第一個程式碼儲存格（表單介面）。
   - 選擇想執行的模型、是否啟用多模態（`LOAD_MMPROJ`）及上下文大小。
   - 點擊執行儲存格，系統將自動配置環境、下載模型並啟動推論伺服器與 SUMA 橋接服務。
4. **切換模型**：
   - 若要切換模型，**請勿點擊「全部執行」或重設執行階段**。
   - 直接捲動至下方第二個「模型切換」表單，選擇新模型後執行該單元即可。

---

## 🛠️ 輔助管理指令

可在新的程式碼儲存格中執行以下指令進行狀態監控與管理：

```python
# 檢視 SUMA 服務即時連線記錄
show_suma_log()

# 停止 llama.cpp 與 SUMA 背景推論服務
stop_services()
```

---

## ⚠️ 注意事項與安全性

- **目標適用族群**：**本方案是專門設計給免費使用 Google Colab T4 GPU (約 15 GB VRAM) 的使用者（也就是 Colab 免費版）**。內建推薦的模型量化檔（Q4_0、Q5_K_M、UD-Q2_K_XL 等）與設定均已針對 T4 15 GB 記憶體限制進行調校，兼顧推論流暢度與避免 OOM。
- **安全性提醒**：SUMA 服務輸出可能包含私有配對碼或連線 Token，請勿將包含此類敏感資訊的 Notebook 輸出直接公開截圖或分享。
- **Gated / Private 倉庫**：本專案預設採用公開權限模型，不讀取亦不傳輸個人 `HF_TOKEN`。若目標倉庫為授權限制模型將會中斷，避免未經授權存取。
- **檔案留存**：模型及環境安裝於 `/content/llama-direct-b12097`，在同一個 Colab 執行階段中保留，換模型時不會刪除既有 GGUF，以利快速切換回原模型。

---

## 📄 授權條款

本專案依照 [MIT License](LICENSE) 規範開源授權。
各模型權重均受其原始開源許可協議規範（如 Gemma、Qwen 授權協議）。
