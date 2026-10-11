# AI-Powered Automated Waste Sorting System

An embedded computer vision prototype designed to automatically identify, classify, and sort plastic types (PET, HDPE, PP, PS) in real-time using a Raspberry Pi 5. Developed as an applied engineering project at ENSEA to optimize recycling workflows and support operators.

---

## Project Overview
Manual sorting in recycling facilities is often slow, physically demanding and prone to human error. This project explores an edge-computing solution to automate the classification and physical separation of 4 common types of plastics:
* **PET** (Polyethylene Terephthalate)
* **HDPE** (High-Density Polyethylene)
* **PP** (Polypropylene)
* **PS** (Polystyrene)

---

## System Architecture & Hardware Stack
The system relies on a complete, low-cost embedded hardware chain ("perception -> calculation -> synchronization -> actuation"):
* **Core Unit:** Raspberry Pi 5 (8 GB RAM) running local model inference.
* **Vision Sensor:** Raspberry Pi Camera Module 3 (Sony IMX708, 12MP with autofocus) housed in a controlled illumination zone.
* **Position Tracking:** Grove Ultrasonic sensors (using time-of-flight measurements) placed at each sorting lane to detect physical object presence.
* **Power & Actuation:** 12V Mini Linear Actuators (100mm stroke, 150mm/s) driven via a 2-Relay Module with optical isolation.

---

## Engineering Methodology & Workflow

1. **Weakly Supervised Annotation & Dataset:** 
   * Utilized a 11,252-image dataset from Kaggle (*Type of Plastic Waste*) covering the 4 target classes.
   * Developed a custom Python script for automated label generation (weak supervision heuristic) to rapidly format data for YOLO.
2. **Deep Learning Model (Edge AI):** 
   * Trained a lightweight YOLOv8 model via Transfer Learning on Google Colab (Tesla T4) using a fine-tuning approach.
   * Optimized performance for real-time edge constraints by shifting to a compact classification structure to eliminate latency on the Raspberry Pi.
3. **FIFO Queue & Ultrasonic Synchronization:** 
   * Implemented a strict FIFO (First-In, First-Out) data structure in Python to store identified plastic types sequentially.
   * Ensured mechanical actuation occurs only when the ultrasonic sensor physically detects the object at the exact drop-off zone, filtering out uncertain predictions via a confidence threshold (>80%)[cite: 10, 17, 36].

---

## Key Results & Performance
* **High Accuracy:** Achieved validation metrics with near **98.4% precision** and a mAP@0.5 of ~0.994 on the dataset.
* **Real-Time Edge Inference:** Successfully validated autonomous execution locally on the Raspberry Pi 5 without external cloud dependency.
* **Real-World Insights:** Identified physical limitations related to lighting reflections and plastic packaging deformations (e.g., brand labels obscuring material features), providing clear pathways for future industrial improvements.

---

## Repository Structure & Case Study
Since proprietary source scripts are archived, this repository serves as an **Engineering Case Study** outlining the system design choices, hardware schematics, and experimental results derived from our official ENSEA project report.

*(You can insert photos of your physical white-wood prototype and wiring schematics here)*
