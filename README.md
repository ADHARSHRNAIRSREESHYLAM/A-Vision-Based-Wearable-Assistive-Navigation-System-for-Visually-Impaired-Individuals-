# Vision-Based Wearable Assistive Navigation System

A lightweight, standalone, voice-controlled wearable indoor navigation device for visually impaired individuals. The system leverages 360-degree LiDAR distance signature matching for indoor localization, front-facing ultrasonic sensors for immediate obstacle detection, and an offline voice interface for hands-free navigation instructions.

Published in the **2026 2nd International Conference on Innovations in Mechanical, Robotics, Computing and Biomedical Engineering (ICIMRBE)**.

---

## 📌 Features

* **Standalone Indoor Localization:** Uses RPLiDAR distance signatures matched against pre-recorded room profiles to identify indoor locations without external infrastructure or GPS.
* **Voice-Controlled Operation:** Integrates the offline VOSK Speech Recognition Engine and Rasa NLU for natural language speech input.
* **Real-Time Obstacle Avoidance:** Continuously scans the forward path using LiDAR and ultrasonic sensors, issuing immediate audio alerts upon obstacle detection.
* **Auditory Guidance:** Delivers step-by-step navigation cues via a Text-to-Speech (TTS) engine through an audio jack.
* **Ergonomic Wearable Design:** Built with a custom 3D-printed PLA housing mounted on a chest harness, weighing ~380g.

---

## 🏗️ Hardware Architecture

The core hardware is housed in a compact $120 \text{ mm} \times 85 \text{ mm} \times 65 \text{ mm}$ 3D-printed enclosure:

* **Processing Hub:** Raspberry Pi 4B
* **2D LiDAR:** RPLiDAR A1 M8 (mounted at top for 360° unobstructed scan)
* **Proximity Sensor:** HC-SR04 Ultrasonic Sensor (front-facing for low/close obstacle detection)
* **Voice Input:** USB Microphone
* **Audio Output:** 3.5mm Audio Jack / Headphones
* **Power Source:** Power bank / Portable Battery Pack

---

## ⚙️ Software Architecture & Workflow

```
[ USB Mic ] ────► Audio Stream ───► [ VOSK ASR Engine ] ──► [ Rasa NLU Core ]
                                                                   │
[ RPLiDAR ] ────► Raw Scans ─────► [ Signature Matcher ] ──────────┤
                                                                   ▼
[ Speaker ] ◄──── Audio Cues ────► [ TTS (eSpeak) ] ◄──── [ Navigation Engine ]
```

1. **System Initialization:** Configures sensor interfaces (GPIO/USB) and initializes VOSK speech recognition and navigation routines.
2. **Location Estimation:** Scans surroundings with RPLiDAR and matches spatial signatures against room profile templates.
3. **Intent Parsing:** Captures user destination via microphone, converts speech to text (VOSK), and parses destination entities (Rasa NLU).
4. **Route Execution & Guidance:** Computes travel paths and outputs step-by-step audio prompts.
5. **Obstacle Monitoring:** Runs an active 0.5s loop; halts navigation and issues voice warnings if an obstacle enters safe threshold distance.

---

## 🚀 Getting Started & Installation

### Prerequisites
* **Hardware:** Raspberry Pi 4B, RPLiDAR A1 M8, HC-SR04 Ultrasonic Sensor, USB Microphone, Audio Output Device
* **OS:** Raspberry Pi OS (64-bit recommended)
* **Python Environment:** Python 3.8+

### Quick Installation Steps

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/vision-based-wearable-navigation.git
   cd vision-based-wearable-navigation
   ```

2. **Install System Dependencies:**
   ```bash
   sudo apt-get update
   sudo apt-get install -y espeak libasound2-dev portaudio19-dev
   ```

3. **Install Python Libraries:**
   ```bash
   pip install numpy pyserial sounddevice vosk rplidar-python pyttsx3 RPi.GPIO rasa
   ```

4. **Download Speech Recognition Model:**
   Download the lightweight VOSK offline model (`vosk-model-small-en-us-0.15`) and extract it into the `model/` folder:
   ```bash
   mkdir -p model
   cd model
   wget https://alphacephei.com/vosk/models/vosk-model-small-en-us-0.15.zip
   unzip vosk-model-small-en-us-0.15.zip
   mv vosk-model-small-en-us-0.15 vosk-model
   cd ..
   ```

5. **Run the System:**
   ```bash
   python3 src/main.py
   ```

---

## 📊 Experimental Results

| Module | Tested Condition / Metric | Result |
| :--- | :--- | :--- |
| **Localization Accuracy** | Room Signature Matching Confidence (Automation Lab) | **92%** confidence (78% under $0.2\sigma$ Gaussian noise) |
| **Voice Interface** | Word Error Rate (WER) in typical indoor noise ($30\text{ dB} - 50\text{ dB}$) | **< 12% WER** |
| **Obstacle Avoidance** | Near-field obstacle distance trigger | Immediate voice alert and safe stop |
| **System Completion** | End-to-end route execution | Destination reached successfully |

---

## 📁 Repository Structure

```
├── Published IEEE paper.pdf          # Full IEEE research paper publication
├── components (1) (1).jpg           # System hardware hardware assembly diagram
├── confidence heatmap (1).jpg       # Probabilistic spatial localization confidence heatmap
├── FLOW CHART (1).jpg              # System control flow & execution state machine
├── speech recognition (1).jpg      # Speech recognition noise performance graph (WER vs dB)
├── ultrasonic graph (1).jpeg       # Ultrasonic sensor distance plot showing obstacle clearance
└── README.md                         # Project documentation
```

---

## 📄 Citation

If you find this project useful in your research or applications, please cite our IEEE conference paper:

```bibtex
  title={A Vision Based Wearable Assistive Navigation System for Visually Impaired Individuals},
  author={Jose, Julia Tholath and Nair, Adharsh R and C, Sneha and Andaluri, Subha Sankar and Payyapilly, Kevin Jojo and M, Athira},
  booktitle={2026 2nd International Conference on Innovations in Mechanical, Robotics, Computing and Biomedical Engineering (ICIMRBE)},
  pages={1--6},
  year={2026},
  organization={IEEE},
  doi={10.1109/ICIMRBE70745.2026.11709990}
}
```
