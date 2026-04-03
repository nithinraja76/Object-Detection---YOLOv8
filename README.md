# Object Detection — YOLOv8

## 📌 Project Overview

A custom-trained YOLOv8 object detection model built to accurately identify 39 unique real-world objects across varied angles, backgrounds, and lighting conditions. The end goal is automated student attendance tracking by mapping detected object IDs to student NUIDs.

---

## 📂 Dataset

| Property | Details |
|---|---|
| Total Classes | 39 unique objects |
| Samples per Class | ~100 images |
| Total Training Images | ~4,000 |
| Image Size | 224 × 224 JPEG |
| Annotation Tool | RoboFlow |
| Export Format | PyTorch (normalized bounding box coordinates) |

**Notes on data quality:**
- Some objects had compressed, out-of-focus, or inconsistent images
- No validation split was provided — 20% of training data was manually held out for validation
- Augmentation was layered: RoboFlow built-in adjustments stacked on top of YOLOv8's native Mosaic and Mixup techniques

---

## 🏗️ Model Architecture

- **Model**: YOLOv8 (Oriented Bounding Box — OBB variant)
- **Annotation**: Vector-based bounding boxes via RoboFlow
- **Augmentation**: RoboFlow preprocessing + YOLOv8 Mosaic & Mixup
- **Validation Strategy**: 80/20 train-validation split (manually extracted)

---

## 📊 Results

| Metric | Score |
|---|---|
| mAP@0.5 | **0.964** |
| Precision | 0.946 |
| Recall | 0.957 |
| F1 Score (peak) | 0.95 @ confidence 0.68 |
| Box Loss | 0.403 |
| Class Loss | 0.308 |

- Training converged steadily with loss dropping each epoch and mAP climbing consistently
- Validation scores plateaued around **epoch 80**, after which further training was stopped to avoid wasting GPU resources
- All reported mAP scores are based on images the model **never saw during training**

---

## 🔍 Key Observations

- **Angle generalization**: Stacking two augmentation layers (RoboFlow + YOLOv8 native) significantly improved the model's ability to detect objects at unusual angles
- **Noisy classes**: Despite imperfect training data for objects like OBJ_021 (mixed lotion bottles) and OBJ_006 (hairclip with low contrast backgrounds), the model still detected them successfully in demo
- **Label rendering bug**: In some demo outputs, bounding box labels appeared outside the frame due to large box sizes — a minor rendering issue, not a detection failure

---

## 🚀 Demo

The model was tested on real-world images containing multiple objects simultaneously. Key demo results:

- Correctly detected all 39 object classes in test images
- Confidence scores ranged from **0.82 to 0.97** across detections
- Successfully identified difficult objects like hairclips and mixed lotion bottles

---

## 🔮 Next Steps

- Assign object IDs to student NUIDs/names for attendance tracking
- Fix bounding box label rendering for large detections
- Improve training data quality for low-performing classes (OBJ_021, OBJ_006)
- Deploy model in a real-time tracking pipeline

---

## 🛠️ Tech Stack

- Python
- YOLOv8 (Ultralytics)
- RoboFlow (annotation & augmentation)
- PyTorch
