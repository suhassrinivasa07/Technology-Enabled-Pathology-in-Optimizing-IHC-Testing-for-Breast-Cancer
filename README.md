# 🧬 IHCGenie — Breast Cancer H&E to IHC Conversion & Severity Prediction  
### AI-Powered Histopathology Image Processing System  
*(Learning / Academic Project)*

This project demonstrates an end-to-end workflow for **processing breast cancer histopathology images**, converting **H&E-stained images into IHC-style images**, and performing **cancer severity prediction** using a deep-learning backend.

It is built using **Flask**, **Python**, and **Machine Learning**, with a web UI for uploading and analyzing tissue images.

> ⚠️ Note: This is an academic learning project implemented using concepts and resources from online tutorials, research papers, and open datasets.

---

## 📌 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Architecture](#project-architecture)
- [Folder Structure](#folder-structure)
- [Working Process](#working-process)
- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Future Enhancements](#future-enhancements)
- [Credits / References](#credits--references)

---

## 🧩 Overview

Pathologists routinely rely on Hematoxylin & Eosin (H&E)-stained slides for initial diagnosis, whereas **Immunohistochemistry (IHC)** provides critical biomarker-level insight for treatment planning. Because physical IHC staining is expensive and time-consuming, this project explores a two-phase computational workflow:

### **Phase 1 — Convert H&E → IHC-Style Image**  
Uses a deep-learning image-to-image translation model to synthesize virtual IHC-stained tissue representations from standard H&E inputs.

### **Phase 2 — Predict Cancer Severity**  
Evaluates the synthesized IHC representation and extracted histological features to output a severity classification score.

---

## ⭐ Features

- 🔬 **Upload H&E Image for Analysis** — Supports standard histopathology image uploads through a browser interface
- 🧪 **Generate IHC-Style Output** — Synthesizes virtual IHC staining from uploaded H&E patches
- 📊 **Predict Severity / Classification Score** — Estimates breast cancer severity level from processed tissue features
- 💾 **Save Analysis Sessions in Database** — Logs historical runs, predictions, and file paths in a local database
- 🖼️ **Side-by-Side Before/After View** — Displays original H&E and generated IHC-style outputs together on the results page
- 🌐 **User-Friendly Flask Web Interface** — Clean upload, processing, and report dashboard

---

## 🛠 Tech Stack

| Component | Technology |
|----------|------------|
| Backend | Python, Flask |
| ML / DL | PyTorch / TensorFlow |
| Image Processing | OpenCV, Pillow, NumPy |
| Frontend | HTML, CSS, JavaScript, Jinja2 Templates |
| Database | SQLite / MySQL |
| Deployment | Localhost (Flask) |

---

## 🏗 Project Architecture

```text
User H&E Upload ──► Image Preprocessing ──► H&E → IHC Translation Model ──► Severity Predictor ──► DB Logging & Report UI
```

---

## 📁 Folder Structure

```text
IHCGenie/
├── app.py
├── routes.py
├── utils.py
├── models/                  # Pretrained H&E→IHC & severity model weights (gitignored)
├── static/
│   ├── css/
│   ├── js/
│   └── images/
├── templates/
│   ├── home.html
│   ├── upload.html
│   └── results.html
├── database/
│   └── sessions.db
├── uploads/
├── outputs/
├── README.md
├── requirements.txt
└── .gitignore
```

---

## 🔄 Working Process

1. User uploads an H&E histopathology image through the web UI.
2. Backend validates and saves the input file into `/uploads/`.
3. The image-translation model converts the H&E input into an IHC-style image saved in `/outputs/`.
4. The severity prediction model evaluates the generated representation and computes a classification score.
5. Results are stored in the database and rendered side-by-side on the results page.

---

## 🧪 Installation

### 1️⃣ Clone this repository
```bash
git clone [https://github.com/suhassrinivasa07/Technology-Enabled-Pathology-in-Optimizing-IHC-Testing-for-Breast-Cancer.git](https://github.com/suhassrinivasa07/Technology-Enabled-Pathology-in-Optimizing-IHC-Testing-for-Breast-Cancer.git)
cd Technology-Enabled-Pathology-in-Optimizing-IHC-Testing-for-Breast-Cancer
```

### 2️⃣ Install dependencies
```bash
pip install -r requirements.txt
```

### 3️⃣ Add pretrained model weights
Place your trained model files inside:
```text
models/
```
*(Model weight files are excluded from Git tracking due to file size limits.)*

---

## ▶️ Running the Application

### 4️⃣ Start the Flask server
```bash
python app.py
```
*(Or `python main.py` depending on your entrypoint file.)*

### 5️⃣ Open in your browser
```text
[http://127.0.0.1:5000](http://127.0.0.1:5000)
```

---

## 🚀 Future Enhancements

- ☁️ **Cloud Deployment** — Containerize and deploy on Render, AWS, or Azure
- 🧬 **Multi-Biomarker IHC Synthesis** — Support separate ER, PR, and HER2 virtual staining channels
- 📈 **Enhanced Translation Fidelity** — Integrate perceptual/stain-consistency losses for sharper cellular boundaries
- ⏳ **Asynchronous Progress Indicator** — Add live upload and inference progress feedback in the UI
- 🔍 **Whole-Slide Image (WSI) Tiling** — Support patch-based tiling and stitching for gigapixel pathology slides

---

## 📚 Credits / References

- **Web & ML Frameworks:** Flask documentation, PyTorch / TensorFlow, OpenCV, and Pillow
- **Domain Literature:** Virtual H&E-to-IHC stain translation and computational histopathology research papers
- **Datasets & Tutorials:** Public breast histopathology datasets and Roboflow / YouTube computer vision references
