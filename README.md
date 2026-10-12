# AI Project — Automated Plastic Waste Sorting System

An embedded computer vision prototype designed to automatically identify, classify, and sort plastic types (PET, HDPE, PP, PS) in real-time on a Raspberry Pi 5.

| | |
| :--- | :--- |
| **Status** | Completed — Embedded hardware prototype and local inference validated |
| **Context** | Academic — Project Semester, ENSEA, 2026 |
| **Team** | Kelvin Raud, Artsrun Sargsyan |
| **My contribution** | Embedded architecture, computer vision pipeline (OpenCV/YOLOv8), Python control logic, ultrasonic synchronisation and hardware prototype integration |
| **Evidence** | [Report (FR, PDF)](./TRI_DES_DECHETS_RAPPORT_SARSGYAN_RAUD.pdf) |

---

## Project Goal

The objective was to design and program an autonomous embedded system capable of classifying plastic waste on a conveyor belt and triggering physical sorting.

The system needed to:

* Identify 4 types of plastics: **PET**, **HDPE**, **PP**, and **PS**
* Process video frames in real-time with low latency on edge hardware
* Synchronize classification decisions with physical object presence
* Actuate linear pistons to eject plastics into dedicated bins

---

## Hardware Platform

The project used a Raspberry Pi 5 core coupled with custom actuation and sensing hardware:

<img width="580" height="679" alt="image" src="https://github.com/user-attachments/assets/4c5f2af5-2904-40be-8f05-888f588823eb" />

*Figure 1 — Schematic of the Automated Plastic Sorting System Prototype*

<img width="428" height="570" alt="image" src="https://github.com/user-attachments/assets/0a1f8398-fb2f-451b-a238-ac59bb4456a7" />

*Figure 2 — Automated Plastic Sorting System Prototype*

---

## Hardware Components

* Raspberry Pi 5 (8GB RAM) central computing unit
* Raspberry Pi Camera Module 3 (Sony IMX708, 12MP, autofocus)
* Grove Ultrasonic distance sensors (time-of-flight presence detection)
* 12V Mini Linear Actuators (100mm stroke, 150mm/s speed)
* 2-Channel Relay Module with optical isolation
* Custom wooden test bay with controlled lighting

---

## Software & Programming

The software architecture was developed in Python:

* **OpenCV & Picamera2** for real-time video stream capture ($640 \times 480$ BGR)
* **YOLOv8 (Ultralytics)** fine-tuned via Transfer Learning on 11,252 plastic dataset images
* **Custom FIFO Queue Logic** to maintain strict sequential ordering between detection and ejection

An initial attempt was made using YOLOv8 Small, but due to inference latency on edge hardware, the final implementation used a compact **YOLOv8 Classification** approach to ensure real-time responsiveness.


## Control Strategy

The sorting pipeline uses a synchronized decision-and-trigger flow:

<img width="928" height="475" alt="image" src="https://github.com/user-attachments/assets/9634963a-c3fb-48a8-8f95-03d6da2ef0e9" />

*Figure 3 — Flowchart of the automated vision-based sorting process*

1. **Vision Inference:** The camera captures frames and outputs a plastic class with a confidence score.
2. **Confidence Filter:** If confidence exceeds **80%**, the label is added to a FIFO queue; otherwise, it is ignored (*unknown*).
3. **Ultrasonic Presence Check:** Sensors continuously measure distance ($0 < \text{Distance} \le 15\text{ cm}$).
4. **Targeted Actuation:** Upon object arrival at the sorting lane, the corresponding linear piston deploys, pauses for ejection, and retracts.

---

## Results

<img width="770" height="433" alt="image" src="https://github.com/user-attachments/assets/fef7e2e7-afaf-44ca-98b4-b5e87762e1b7" />

*Figure 4 — PET detection - Bottle of juice *

<img width="1920" height="1080" alt="20260116_11h19m15s_grim" src="https://github.com/user-attachments/assets/93e37b16-25f9-46e5-82bb-28108c65fa33" />


*Figure 5 — HDPE detection - Detergent bottle *

<img width="1920" height="1080" alt="20260116_11h19m34s_grim" src="https://github.com/user-attachments/assets/b1494c94-5f65-4c75-8858-d5b8e4f71c2d" />

*Figure 5 — PP detection - Sandwich plastic box *

<img width="1920" height="1080" alt="20260116_11h20m50s_grim" src="https://github.com/user-attachments/assets/2dad8162-e93d-48be-ac03-20ab99467092" />

*Figure 5 — PS detection - Cap *

The prototype successfully validated the complete vision-to-actuation chain:

1. Real-time recognition of PET bottles with up to **100% confidence**
2. Successful sequential queuing and synchronized piston triggering
3. Autonomous local inference without cloud dependencies

However, plastic items with dominant brand labels (e.g., HDPE bottles covered in opaque plastic sleeves) occasionally misclassified when material features were hidden from the camera. As stated in the report, future work includes enriching the dataset with labeled packaging variants and integrating a motorized conveyor belt.
