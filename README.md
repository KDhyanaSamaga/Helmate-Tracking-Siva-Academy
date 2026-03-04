<div align="center">

# 🪖 Helmet Detection using YOLOv8

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-EE4C2C?style=flat-square)](https://ultralytics.com)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)](https://colab.research.google.com)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=flat-square)](LICENSE)

*A real-time helmet detection system built with YOLOv8, trained on a custom dataset to identify helmet compliance in traffic and construction scenarios.*

</div>

---

## 📌 Overview

This project fine-tunes a **YOLOv8 nano model** on a custom helmet dataset to detect whether individuals are wearing helmets or not. The model is trained and evaluated entirely on **Google Colab** using the Ultralytics framework.

**Use cases:**
- Traffic surveillance (two-wheeler helmet compliance)
- Construction site safety monitoring
- Automated PPE (Personal Protective Equipment) detection

---

## 🗂️ Project Structure

```
helmet-detection/
├── data.yaml               # Dataset config (classes, paths)
├── Helmate.v1i.yolov8.zip  # Roboflow exported dataset
├── Helmate_tracking.ipynb  # Main training notebook
├── traffic.webp            # Sample test image
├── runs/
│   └── detect/
│       └── train/          # Training outputs (weights, metrics)
│           ├── weights/
│           │   ├── best.pt
│           │   └── last.pt
│           └── results.png
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/helmet-detection.git
cd helmet-detection
```

### 2. Install Dependencies
```bash
pip install ultralytics
```

> If running on **Google Colab**, simply execute the cell — CUDA and OpenCV come pre-installed.

### 3. Prepare the Dataset

The dataset is sourced from **Roboflow** in YOLOv8 format. Unzip it before training:

```python
!unzip Helmate.v1i.yolov8.zip
```

Your `data.yaml` should look like:

```yaml
train: /content/train/images
val:   /content/valid/images
test:  /content/test/images

nc: 2
names: ['helmet', 'no-helmet']
```

---

## 🚀 Training

```python
from ultralytics import YOLO

# Load YOLOv8 nano pretrained on COCO
model = YOLO('yolov8n.pt')

# Fine-tune on helmet dataset
model.train(data='/content/data.yaml', epochs=100)
```

**Training config at a glance:**

| Parameter | Value |
|-----------|-------|
| Base model | YOLOv8n (nano) |
| Epochs | 100 |
| Dataset format | YOLOv8 (Roboflow) |
| Platform | Google Colab (T4 GPU) |

> **Note:** YOLOv8n is the lightest variant — ideal for quick experimentation. Swap to `yolov8s.pt` or `yolov8m.pt` for higher accuracy at the cost of speed.

---

## 📊 Evaluation

```python
metrics = model.val()
```

This outputs key metrics on the validation set:

| Metric | Description |
|--------|-------------|
| **mAP@50** | Mean Average Precision at IoU 0.50 |
| **mAP@50-95** | Stricter mAP across IoU thresholds |
| **Precision** | Of all detections, how many were correct |
| **Recall** | Of all actual helmets, how many were found |

---

## 🖼️ Inference

Run prediction on a single image:

```python
import cv2
from ultralytics import YOLO
from google.colab.patches import cv2_imshow

model = YOLO('runs/detect/train/weights/best.pt')  # Use best trained weights

path = '/content/traffic.webp'
results = model(path)
results[0].show()
```

To save the annotated output:

```python
results = model(path, save=True)
# Saved to runs/detect/predict/
```

---

## 📦 Dataset

Dataset created and exported using **[Roboflow](https://roboflow.com)** in YOLOv8 format.

- **Classes:** `helmet`, `no-helmet`
- **Format:** YOLOv8 (images + labels in `.txt`)
- **Splits:** Train / Valid / Test

> To use your own dataset, replace `Helmate.v1i.yolov8.zip` with your Roboflow export and update `data.yaml` paths accordingly.

---

## 🔧 Customization

| What to change | Where |
|---------------|-------|
| Number of training epochs | `model.train(epochs=...)` |
| Model size | Replace `yolov8n.pt` → `yolov8s/m/l/x.pt` |
| Input image size | `model.train(imgsz=640)` |
| Confidence threshold | `model(path, conf=0.5)` |
| Dataset classes | `data.yaml → names` |

---

## 📈 Results

Training results (loss curves, mAP plots) are auto-saved to:

```
runs/detect/train/results.png
```

Sample predictions are saved to:

```
runs/detect/predict/
```

---

## 🛠️ Tech Stack

- **[Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)** — Object detection framework
- **[Roboflow](https://roboflow.com)** — Dataset management and export
- **[Google Colab](https://colab.research.google.com)** — Cloud GPU training
- **OpenCV** — Image processing and visualization

---

## Author

<div align="center">

**K Dhyana Samaga**
**Freelance Trainer**
[![Medium](https://img.shields.io/badge/Medium-@kdhyanasamaga-000000?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@kdhyanasamaga)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com)

</div>

---

<div align="center">
<sub>If this project helped you, consider leaving a ⭐</sub>
</div>
