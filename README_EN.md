# v2.3.4 SUMA Remote Model Launcher (Colab llama.cpp Launcher)

[English](README_EN.md) | [繁體中文](README.md)

> **Current Engine Version**: `llama.cpp b12651` | **SUMA Remote AI Bridge**: `0.3.6.0 Beta`

A high-performance launcher and model switcher for Google Colab, built on official precompiled `llama.cpp` binaries. Supports GGUF models, multimodal vision projection (MMProj), dynamic context switching, and leverages **[SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) technology** to seamlessly access remote Colab GPU compute resources.

---

## ⚡ Open in Google Colab

| Edition | Target Hardware | Notebook | Launch |
| :--- | :--- | :--- | :---: |
| **English (Standard)** | T4 GPU (15 GB VRAM / Free Tier) | `colab-llamacpp-hub_EN.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nkrf9a1eVHEzES_QU5y00vf365FPwgnK?usp=sharing) |
| **English (96G)** | High-VRAM GPU (96 GB VRAM) | `colab-llamacpp-hub_96G_EN.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/10Em-w1Y7xRSf7N56UBOc4_aENPjjoQJU?usp=sharing) |
| **Traditional Chinese (Standard)** | T4 GPU (15 GB VRAM / Free Tier) | `colab-llamacpp-hub.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nec-U8HF8nX17F5zZrImr5xj5VeFY11v?usp=sharing) |
| **Traditional Chinese (96G)** | High-VRAM GPU (96 GB VRAM) | `colab-llamacpp-hub_96G.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1PlOqL4JMTxaWYlyU2m4rdDwlmoE0yv5C?usp=sharing) |

---

## 🌟 Key Features

- **[SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) Integration**: Powered by [SUMA Remote AI Bridge](https://suma.tw/product/ai-bridge/) technology to securely bridge remote Google Colab GPU computing power directly to local clients and applications without requiring public IP configuration, port forwarding, or complex reverse proxies.
- **Zero-Build Instant Setup**: Downloads and extracts official prebuilt `llama.cpp` (b12651) binaries directly, eliminating long build and compilation times.
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
| **Qwen3.5-9B Q5_K_M** | `unsloth/Qwen3.5-9B-GGUF` | `qwen3.5-9b` | ✅ Supported |
| **Qwen3.8-27B UD-Q2_K_XL** | `unsloth/Qwen3.8-27B-GGUF` | `qwen3.8-27b-q2kxl` | ✅ Supported |
| **Inkling-2-Mini UD-Q3_K_XL** | `unsloth/Inkling-2-Mini-GGUF` | `inkling-2-mini-q3kxl` | ✅ Supported |

### Model Highlights

1. **Gemma 4 12B QAT Q4_0**
   - **Features**: Google's latest lightweight, high-efficiency instruction-tuned model processed with Quantization-Aware Training (QAT), balancing low VRAM footprint with high accuracy; supports multimodal vision projection (MMProj).
   - **Native Context**: Up to 256K tokens (Launcher supports up to 256K).

2. **Qwen3.5-9B Q5_K_M**
   - **Features**: Alibaba's versatile lightweight multilingual model using Q5_K_M medium-high precision quantization, offering smooth inference, strong Chinese comprehension, and balanced reasoning; supports MMProj for vision understanding.
   - **Native Context**: Up to 262,144 tokens (256K; Launcher supports up to 256K).

3. **Qwen3.8-27B UD-Q2_K_XL**
   - **Features**: A 27B parameter model utilizing Unsloth Dynamic 2-bit ultra-low-bit quantization (UD-Q2_K_XL), enabling 16GB VRAM GPUs (such as Colab T4) to run large-scale reasoning; supports multimodal vision projection.
   - **Native Context**: Up to 262,144 tokens (256K; Launcher supports up to 256K).

4. **Inkling-2-Mini UD-Q3_K_XL**
   - **Features**: Thinking Machines' MoE multimodal model with Unsloth Dynamic 3.0 UD-Q3_K_XL quantization, balancing low VRAM usage, strong reasoning, and vision capabilities.
   - **Native Context**: Up to 256K tokens (Launcher supports up to 256K).

* Context Window Options: `64K`, `128K`, `256K` (Please select based on your allocated Colab GPU VRAM to avoid Out-Of-Memory errors).

---

## 🚀 Quick Start

1. **Open the Notebook**: Upload or import `colab-llamacpp-hub_EN.ipynb` into Google Colab, or simply click the launch link in the **Open in Google Colab** table above.
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
- **Persistent Cache**: Files are stored in `/content/llama-direct-b12097`. Reusing the same Colab runtime preserves downloaded models for rapid switching.

---

## 📄 License

This project is released under the [MIT License](LICENSE).
Individual model weights are governed by their respective upstream licenses (e.g., Google Gemma, Alibaba Qwen).
