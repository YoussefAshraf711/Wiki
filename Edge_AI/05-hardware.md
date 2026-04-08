<div align="center">

# 🔧 Part 5: Hardware & Inference Frameworks

**The silicon and software that make edge AI possible — from NVIDIA Jetson to Apple Neural Engine.**

`⏱ 8 min read` · `📊 Intermediate` · `📱 Edge AI Masterclass 5/8`

</div>

---

## 📌 Quick Summary

> Edge AI runs on specialized hardware (NPUs, GPUs, TPUs) using optimized inference frameworks (ONNX Runtime, TensorFlow Lite, llama.cpp). The hardware determines your power envelope and compute ceiling. The framework determines how efficiently you use that hardware.

---

## 🖥️ The Hardware Landscape

### Edge AI Accelerators Compared:

| Hardware | Type | Performance | Power | Price | Best For |
|:--|:--|:--|:--|:--|:--|
| **Apple Neural Engine** | NPU (in M-series/A-series) | 38 TOPS (M4) | ~5W | Built into Apple devices | iPhones, iPads, Macs |
| **NVIDIA Jetson Orin Nano** | GPU module | 40 TOPS | 15W | ~$250 | Robots, drones, IoT |
| **Google Coral Edge TPU** | TPU accelerator | 4 TOPS | 2W | ~$60 | Low-power IoT |
| **Qualcomm Hexagon NPU** | NPU (in Snapdragon) | 45 TOPS (8 Gen 3) | ~5W | Built into Android phones | Smartphones |
| **Intel Movidius** | VPU | 1 TOPS | <1W | ~$80 | Ultra-low power vision |
| **Raspberry Pi 5 + Hailo-8** | AI HAT module | 13 TOPS | 5W | ~$100 | Hobbyist, education |

### Key Metric: TOPS (Tera Operations Per Second)
```
1 TOPS = 1 trillion operations per second

Context:
  Running a 1B parameter model inference ≈ 2 TOPS
  Running a 7B parameter model inference ≈ 14 TOPS
  Real-time video object detection     ≈ 5 TOPS
```

---

## 🛠️ Inference Frameworks

| Framework | Runs On | Language | Best For |
|:--|:--|:--|:--|
| **llama.cpp / Ollama** | CPU, Apple Metal, CUDA | C/C++ | LLMs on CPU (laptops, servers) |
| **ONNX Runtime** | CPU, GPU, NPU, TPU | Python/C++ | Cross-platform model deployment |
| **TensorFlow Lite** | Mobile, IoT | Python/Java/C++ | Android/iOS apps, microcontrollers |
| **Core ML** | Apple devices only | Swift/Python | iOS, iPadOS, macOS apps |
| **NVIDIA TensorRT** | NVIDIA GPUs only | C++/Python | Maximum GPU inference speed |
| **MediaPipe** | Mobile, Web, Desktop | Python/JS | Pre-built vision/NLP tasks |
| **MLC LLM** | Phone, Laptop, Web | Python/C++ | LLMs on any device via compilation |

### Decision Guide:

```
"Running LLMs on a laptop (CPU)?"
  → llama.cpp / Ollama

"Deploying to mobile apps?"
  → TensorFlow Lite (Android) or Core ML (iOS)

"Need cross-platform consistency?"
  → ONNX Runtime

"Have NVIDIA GPU and need max speed?"  
  → TensorRT

"Building a quick prototype?"
  → Ollama (simplest setup)
```

---

## ⚡ Speed Benchmark: Llama 3.2 3B on Different Hardware

| Hardware | Framework | Tokens/Second | Setup Difficulty |
|:--|:--|:--|:--|
| M2 Pro MacBook (CPU) | Ollama | ~25 tok/s | ⭐ Trivial |
| M2 Pro MacBook (GPU) | Ollama (Metal) | ~45 tok/s | ⭐ Trivial |
| NVIDIA RTX 4090 | llama.cpp (CUDA) | ~90 tok/s | ⭐⭐ Easy |
| NVIDIA Jetson Orin Nano | llama.cpp | ~15 tok/s | ⭐⭐⭐ Moderate |
| Pixel 8 Pro (Tensor G3) | MediaPipe LLM | ~12 tok/s | ⭐⭐ Easy |
| Raspberry Pi 5 | llama.cpp | ~3 tok/s | ⭐⭐ Easy |

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 4: Optimization](04-optimization.md) |
| 📑 **Table of Contents** | [Edge AI Masterclass Home](README.md) |
| ➡️ **Next** | [Part 6: TinyML & Microcontrollers →](06-tinyml.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
