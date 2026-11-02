# SUMA Remote Model Launcher (Colab llama.cpp Launcher)

[English](README_EN.md) | [繁體中文](README.md)

A high-performance launcher and model switcher for Google Colab, built on official precompiled `llama.cpp` binaries. Supports GGUF models, multimodal vision projection (MMProj), dynamic context switching, and leverages **SUMA Remote AI Bridge technology** to seamlessly access remote Colab GPU compute resources.

---

## 🌟 Key Features

- **SUMA Remote AI Bridge Integration**: Powered by SUMA Remote AI Bridge technology to securely bridge remote Google Colab GPU computing power directly to local clients and applications without requiring public IP configuration, port forwarding, or complex reverse proxies.
- **Zero-Build Instant Setup**: Downloads and extracts official prebuilt `llama.cpp` (b12020) binaries directly, eliminating long build and compilation times.
- **Form-Based UI**: Utilizes Google Colab Form controls with clean dropdown selections. Code cells are collapsed by default for an intuitive user experience.
- **Seamless Hot Switching**: Separate forms for "Initial Startup" and "Model Switching". Switch models smoothly while terminating prior processes and releasing GPU VRAM without restarting the VM session.
- **Smart MMProj Fallback**: Automatically loads vision projection components when available. If the selected model lacks MMProj support, it gracefully falls back to text-only mode without breaking execution.
- **Resumable Downloads**: Built-in chunked downloading and range-based breakpoint resumption saves time and bandwidth across sessions.

---

## 📋 Supported Models & Specs

Preconfigured with optimized public GGUF models:

| Model Name | Hugging Face Repository | Alias | Multimodal (MMProj) |
| :--- | :--- | :--- | :---: |
| **Gemma 4 12B QAT Q4_0** | `google/gemma-4-12B-it-qat-q4_0-gguf` | `gemma-4-12b` | ✅ Supported |
| **Qwen3.5-9B Q5_K_M** | `unsloth/Qwen3.5-9B-GGUF` | `qwen3.5-9b` | ❌ Text Only |
| **Qwen3.8-27B UD-Q2_K_XL** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-q2kxl` | ❌ Text Only |
| **Qwen3.8-27B UD-IQ3_XXS** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-iq3xxs` | ❌ Text Only |

* Context Window Options: `32K`, `64K`, `128K`, `256K` (Please select based on your allocated Colab GPU VRAM to avoid Out-Of-Memory errors).

---

## 🚀 Quick Start

1. **Open the Notebook**: Import `colab-llamacpp-hub.ipynb` into Google Colab.
2. **Enable GPU Accelerator**: Go to **Runtime** > **Change runtime type**, and select **T4 GPU** (or any higher-tier GPU).
3. **Initial Launch**:
   - Locate the first code cell (Form UI).
   - Select your target model, configure MMProj (`LOAD_MMPROJ`), and choose the context window size.
   - Run the cell. The script will download the runtime, fetch the model, and launch both `llama.cpp` and the SUMA service bridge.
4. **Switch Models**:
   - To change models, **do not click "Run All" or restart the runtime**.
   - Scroll down to the second "Model Switching" form, select your new configuration, and execute only that cell.

---

## 🛠️ Management & Monitoring Commands

Run the following helper functions in a new code cell when needed:

```python
# View real-time SUMA daemon connection logs
show_suma_log()

# Terminate running llama.cpp and SUMA background processes
stop_services()
```

---

## ⚠️ Notes & Security

- **Target Audience**: **This solution is specifically designed for users running on the free tier of Google Colab with a T4 GPU (approx. 15 GB VRAM)**. The default model quants (e.g., Q4_0, Q5_K_M, UD-Q2_K_XL) and context options are tuned specifically for T4 memory constraints to maintain smooth inference and avoid Out-Of-Memory (OOM) crashes.
- **Sensitive Outputs**: SUMA connection logs may output pairing tokens or passcodes. Avoid sharing raw notebook outputs or screenshots publicly.
- **Gated / Private Models**: This notebook focuses on public models and does not transmit or read `HF_TOKEN`. Requests to gated or private repositories will stop safely.
- **Persistent Cache**: Files are stored in `/content/llama-direct-b12020`. Reusing the same Colab runtime preserves downloaded models for rapid switching.

---

## 📄 License

This project is released under the [MIT License](LICENSE).
Individual model weights are governed by their respective upstream licenses (e.g., Google Gemma, Alibaba Qwen).
