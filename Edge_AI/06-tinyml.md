<div align="center">

# 🔬 Part 6: TinyML — AI on Microcontrollers

**Running machine learning on devices with 256 KB of RAM and a coin-cell battery.**

`⏱ 8 min read` · `📊 Advanced` · `📱 Edge AI Masterclass 6/8`

</div>

---

## 📌 Quick Summary

> **TinyML** is the practice of running machine learning models on microcontrollers (MCUs) — devices with as little as 256 KB of RAM and milliwatt power budgets. These models handle always-on tasks like keyword detection ("Hey Siri"), gesture recognition, and anomaly detection, running for months or years on a single battery.

---

## 🔋 What Makes TinyML Special

| Spec | Regular Edge AI | TinyML |
|:--|:--|:--|
| **Device** | Smartphone, laptop, Jetson | Arduino, ESP32, STM32 |
| **RAM** | 2-16 GB | 256 KB - 2 MB |
| **Storage** | 32-512 GB | 1-16 MB flash |
| **Power** | 5-45W | 1-100 milliwatts |
| **Battery life** | Hours | Months to years |
| **Model size** | 500 MB - 28 GB | 10 KB - 500 KB |
| **Use case** | Text generation, image classification | Keyword detection, vibration anomaly |

---

## 🏗️ The TinyML Deployment Pipeline

```
┌──────────────────┐
│ 1. Train Model   │  ← On your laptop/cloud (PyTorch/TensorFlow)
│    (Full-size)   │     e.g., Keyword detection model
└────────┬─────────┘
         │
┌────────▼─────────┐
│ 2. Convert        │  ← TensorFlow Lite Converter / ONNX
│    to TFLite      │     Reduces from 10 MB to 50 KB
└────────┬─────────┘
         │
┌────────▼─────────┐
│ 3. Quantize       │  ← INT8 quantization (full integer)
│    to INT8        │     Further reduces to 15 KB
└────────┬─────────┘
         │
┌────────▼─────────┐
│ 4. Compile to C   │  ← xxd or tflite_micro tools
│    Array           │     Model becomes a C byte array
└────────┬─────────┘
         │
┌────────▼─────────┐
│ 5. Flash to MCU   │  ← Upload via USB/JTAG
│    (Arduino/ESP32) │     Model runs on bare metal
└──────────────────┘
```

---

## 🎯 Common TinyML Applications

| Application | Sensors | Model Type | Power Budget | Example |
|:--|:--|:--|:--|:--|
| **Keyword Detection** | Microphone | CNN/RNN | <1 mW | "Hey Siri", "OK Google" |
| **Gesture Recognition** | Accelerometer | CNN | <0.5 mW | Wrist gesture for smartwatch |
| **Anomaly Detection** | Vibration sensor | Autoencoder | <2 mW | Detect motor/pump failure |
| **Environmental Sensing** | Temp, humidity, air quality | Random Forest | <0.1 mW | Smart building HVAC |
| **Visual Wake Words** | Camera | MobileNet | <10 mW | Person detection (is someone there?) |

---

## 🛠️ TinyML Frameworks

| Framework | Target Hardware | Language | Maintained By |
|:--|:--|:--|:--|
| **TensorFlow Lite Micro** | Any Cortex-M MCU | C++ | Google |
| **Edge Impulse** | Arduino, ESP32, Nordic | Visual + C++ | Edge Impulse Inc. |
| **TinyEngine** | ARM Cortex-M | C | MIT HAN Lab |
| **microTVM** | Any MCU/RTOS | Python + C | Apache TVM |

### Edge Impulse (Easiest Way to Start):
Edge Impulse provides a web-based interface where you:
1. Upload sensor data (audio, accelerometer, images)
2. Train a model with one click
3. Download optimized firmware for your specific board
4. Flash it to your Arduino/ESP32

No ML expertise required to get started.

---

<div align="center">

| Navigation | |
|:--|:--|
| ⬅️ **Previous** | [Part 5: Hardware](05-hardware.md) |
| 📑 **Table of Contents** | [Edge AI Masterclass Home](README.md) |
| ➡️ **Next** | [Part 7: Security & Privacy →](07-security-use-cases.md) |

</div>

---
<div align="center">
<sub>Part of the <a href="../README.md">AI Engineering Wiki</a> · Created by Youssef Ashraf · 2026</sub>
</div>
