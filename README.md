# SUMA 遠端模型啟動器 (Colab llama.cpp Launcher)

[English](README_EN.md) | [繁體中文](README.md)

專為 Google Colab 設計的高效模型啟動與切換工具，基於 `llama.cpp` 官方預編譯二進位檔構建，支援 GGUF 格式、多模態（MMProj）、動態上下文長度切換，並**透過 SUMA Remote AI Bridge 技術無縫存取遠端 Colab GPU 算力**。

---

## 🌟 核心特色

- **SUMA Remote AI Bridge 算力串接**：採用專利級的 SUMA Remote AI Bridge 技術，直接將遠端 Google Colab 的 GPU 算力安全對接到本地應用與客戶端，無需設定公網 IP、連接埠轉發（Port Forwarding）或繁瑣的反向代理。
- **零建置快速啟動**：直接下載並解壓 `llama.cpp` (b12020) 官方預編譯套件，省去漫長的本機編譯時間。
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
| **Qwen3.5-9B Q5_K_M** | `unsloth/Qwen3.5-9B-GGUF` | `qwen3.5-9b` | ❌ 純文字 |
| **Qwen3.8-27B UD-Q2_K_XL** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-q2kxl` | ❌ 純文字 |
| **Qwen3.8-27B UD-IQ3_XXS** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-iq3xxs` | ❌ 純文字 |

* 上下文長度（Context）可選：`32K`、`64K`、`128K`、`256K`（請依 Colab 實際分配到的 GPU 記憶體配置，避免 OOM）。

---

## 🚀 快速開始

1. **開啟 Notebook**：將 `colab-llamacpp-hub.ipynb` 上傳或匯入至 Google Colab。
2. **啟用 GPU 加速**：在 Colab 頂部選單點選 **執行階段** > **變更執行階段類型**，硬體加速器選擇 **T4 GPU** 或其他可用 GPU。
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
- **檔案留存**：模型及環境安裝於 `/content/llama-direct-b12020`，在同一個 Colab 執行階段中保留，換模型時不會刪除既有 GGUF，以利快速切換回原模型。

---

## 📄 授權條款

本專案依照 [MIT License](LICENSE) 規範開源授權。
各模型權重均受其原始開源許可協議規範（如 Gemma、Qwen 授權協議）。
