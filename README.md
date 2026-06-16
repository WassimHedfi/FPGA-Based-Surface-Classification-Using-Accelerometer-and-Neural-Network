# FPGA-Based-Surface-Classification-Using-Accelerometer-and-Neural-Network

## Overview

This project implements a **real-time surface classification system on FPGA** using onboard accelerometer data and a small neural network.

The system detects vibration patterns generated when tapping different surfaces and classifies them using a **hardware-accelerated MLP (Multi-Layer Perceptron)** deployed on an FPGA.

This project is designed as a **first step toward FPGA-based AI systems**, with applications in embedded AI, edge computing, and future agricultural/medical sensing systems.

---

## Project Goal

Classify surfaces based on vibration signals:

- Wood
- Metal
- Foam

When the FPGA board is tapped on a surface, the system captures accelerometer data and predicts the material in real time.

---

## System Architecture

```text
+----------------------+
| Accelerometer (IMU)  |
+----------------------+
           |
           v
+----------------------+
| Fixed Sampling       |
| (~200 Hz Timer)      |
+----------------------+
           |
           v
+----------------------+
| UART Streaming       |
+----------------------+
           |
           v
+----------------------+
| PC Data Logger       |
| (Python)             |
+----------------------+
           |
           v
+----------------------+
| Feature Extraction   |
+----------------------+
           |
           v
+----------------------+
| NN Training          |
| (PyTorch / Sklearn)  |
+----------------------+
           |
           v
+----------------------+
| Quantized Weights    |
| Export               |
+----------------------+
           |
           v
+----------------------+
| FPGA Inference      |
| Engine (MLP)        |
+----------------------+
           |
           v
+----------------------+
| Output (LEDs/UART)   |
+----------------------+
```
---

## Key Features

- Real-time accelerometer data acquisition  
- UART-based data streaming to PC  
- Automatic dataset generation in CSV format  
- Feature extraction from vibration signals  
- Lightweight neural network (MLP)  
- Fixed-point quantized inference on FPGA  
- LED / UART-based classification output  

---

## Hardware Requirements

- DE10-Lite FPGA board (Intel MAX 10)  
- Onboard accelerometer (SPI-based IMU)  
- USB connection (UART communication to PC)  

---

## Software Requirements

### FPGA Development
- Intel Quartus Prime Lite Edition  
- ModelSim Intel Edition (optional simulation)

### Data Processing & ML
- Python 3.x  
- NumPy  
- Pandas  
- Matplotlib  
- PyTorch or Scikit-learn  

### Communication
- PySerial (UART communication)

---

## Dataset Collection

### Sampling Parameters

- Sampling rate: **200 Hz**  
- Recording duration: **1 second per sample**  
- Channels: **ax, ay, az**

Each sample corresponds to a single tap event.

---

### Data Format

Each recording is stored as a CSV file:
ax, ay, az, label
0.02, -0.01, 0.98, wood
...

---

## Dataset Structure

```text
dataset/
├── wood/
│   ├── wood_001.csv
│   ├── wood_002.csv
│   └── ...
├── metal/
│   ├── metal_001.csv
│   ├── metal_002.csv
│   └── ...
└── foam/
    ├── foam_001.csv
    ├── foam_002.csv
    └── ...
```

---

## Feature Extraction

For each recording, statistical features are extracted:

- Mean  
- Standard deviation  
- Maximum  
- Minimum  
- Energy  

Features are computed for each axis (X, Y, Z).

Final input vector example:
[X_mean, X_std, X_max,
Y_mean, Y_std, Y_max,
Z_mean, Z_std, Z_max]


---

## Neural Network Model

A lightweight MLP is used:
Input layer: 9 features
Hidden layer: 8 neurons (ReLU)
Output layer: 3 classes


### Architecture


9 → 8 → 3


### Output Classes

- 0 → Wood  
- 1 → Metal  
- 2 → Foam  

---

## FPGA Implementation

The neural network is implemented in hardware using:

- Fixed-point arithmetic (INT8)
- Multiply-Accumulate (MAC) units
- On-chip memory (M10K blocks)
- ReLU activation
- ArgMax classifier

---

## Data Flow on FPGA


Accelerometer → SPI Interface → Sampling Timer
→ FIFO Buffer → UART TX
→ Optional FPGA MLP Inference
→ Output LEDs / UART result


---

## UART Communication

- Baud rate: 115200 or 921600  
- Format: 8-N-1  
- Data type: streamed raw accelerometer samples  

---

## Demo Behavior

When running the system:

1. Place board on a surface (wood, metal, or foam)
2. Tap the surface
3. FPGA captures vibration signal
4. Neural network processes features
5. Output is displayed:


Prediction: WOOD
Confidence: 96%


---

## Future Improvements

- Raw signal CNN-based classification  
- ECG / biomedical signal processing  
- Agricultural soil vibration analysis  
- Transformer-based edge AI on FPGA  
- Multi-sensor fusion systems  

---

## Why This Project Matters

This project demonstrates:

- Embedded signal acquisition  
- Real-time data streaming  
- Machine learning pipeline  
- Hardware acceleration  
- Edge AI deployment  

It is a first step toward **FPGA-based intelligent sensing systems**.

---

## Author

Wassim Hedfi
