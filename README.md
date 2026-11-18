# ⛈️ APM-Nowcast — Adaptive Physics-Guided Multimodal Convective Nowcasting  
### Real-Time Thunderstorm, Lightning & Cyclonic Risk Prediction System across Pan-India  
*(Research & Operational Prototype)*

**APM-Nowcast** is an end-to-end spatiotemporal deep-learning and mesoscale physics-guided nowcasting platform designed for **short-range convective initiation, lightning flash density, and cyclonic storm tracking** (`T+0` to `T+60 min`) across the Pan-India domain (`6.0°N–38.0°N, 68.0°E–98.0°E`).

It combines a **PyTorch ConvLSTM / U-Net spatiotemporal backbone** with **FiLM physics conditioning**, **Monte Carlo Dropout uncertainty quantification**, a **5-point spatial stencil + rotational-cancellation steering engine** over live Open-Meteo NWP fields, and an interactive **React + TypeScript geospatial dashboard**.

> ⚠️ Note: This is a research and academic engineering prototype. Outputs reflect neural-physics model inference on multi-modal atmospheric inputs and live NWP fields, not official IMD operational warnings.

---

## 📌 Table of Contents
- [Overview](#overview)
- [Research Contributions](#research-contributions)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Architecture](#project-architecture)
- [Folder Structure](#folder-structure)
- [Working Process](#working-process)
- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Testing & Verification](#testing--verification)
- [Screenshots](#screenshots)
- [Future Enhancements](#future-enhancements)
- [Credits / References](#credits--references)

---

## 🧩 Overview

Severe convective storms, lightning strikes, and tropical cyclones over the Indian subcontinent evolve rapidly at mesoscale resolutions where coarse numerical weather prediction (NWP) models smooth out eyewall gradients and single-sensor pipelines degrade during radar or satellite outages. **APM-Nowcast** addresses this across two integrated layers:

### **Phase 1 — Physics-Guided Multimodal Spatiotemporal Modeling**  
Fuses **DWR Doppler Radar**, **INSAT-3D/3DR/3DS Satellite**, **NRSC Lightning**, and **ERA5 / NWP Thermodynamics** using adaptive cross-modal attention, FiLM physics conditioning (`CAPE`, `CIN`, vertical shear), and Monte Carlo Dropout heads for calibrated probabilistic forecasts (`T+15`, `T+30`, `T+45`, `T+60 min`).

### **Phase 2 — Live Mesoscale & Cyclonic Atmospheric Analyzer**  
Queries a **5-point spatial stencil (`Center + ±1.0° N/S/E/W`)** across multi-level pressure surfaces (`850`, `500`, and `300 hPa`), performs **rotational-cancellation deep-layer steering**, applies **3-node Inverse-Distance Weighted (IDW) spatial interpolation**, and dynamically classifies convective regimes without hardcoded storm rules.

---

## 🔬 Research Contributions

- **RG1 — Adaptive Multimodal Fusion:** Dynamically re-weights attention across Radar, Satellite, Lightning, and NWP streams, cleanly zeroing dropped sensors and renormalizing weights under single- or multi-feed outages.
- **RG2 — Convective Initiation Prediction:** Gates false alerts in dry capped high-`CAPE` air using `700 hPa` relative humidity, `CIN`, and local radial pressure drops (`ΔP`), while capturing tight cyclonic cores and developing cells.
- **RG6 — Uncertainty-Aware Prediction:** Decomposes total forecast uncertainty into **aleatoric** and **epistemic** bounds via Monte Carlo Dropout, inflating confidence intervals proportionally during sensor blackouts.
- **RG7 — Physics-Guided Learning & Inference:** Integrates near-identity FiLM thermodynamic conditioning, physics-constrained loss regularization, and deep-layer (`850/500/300 hPa`) rotational-cancellation steering vectors.

---

## ⭐ Features

- 🗺️ **Interactive Pan-India Geospatial Map** — Real-time convective cells, wind streamlines, DWR 250 km radar range rings, and sub-region zoom presets
- ⚡ **Multi-Horizon Probabilistic Nowcasting (`+15` to `+60 min`)** — Lightning occurrence probability (`%`), flash rate density (`fl/km²`), and total/aleatoric uncertainty bounds
- 🛰️ **Sensor Dropout & Reliability Simulator** — Toggle Radar, Satellite, Lightning, or NWP feeds live to inspect dynamic attention weight redistribution and uncertainty inflation
- 🌀 **Zero-Hardcoding 5-Point Spatial Stencil** — Resolves cyclone eyewalls, radial pressure anomalies (`ΔP`), and deep-layer environmental steering drift (`NNW`/`NE` translation)
- 📍 **3-Node IDW Spatial Interpolation** — Eliminates duplicate telemetry across neighboring inland regions by smoothly interpolating thermodynamic and wind fields
- 🏷️ **Physics-Driven Regime Labeling** — Dynamically classifies cells into `Cyclonic Cell`, `Cyclonic Band`, `Convective Core`, `Developing Cell`, or `Stable Air Mass`

---

## 🛠 Tech Stack

| Component | Technology |
|----------|------------|
| Backend API | Python 3.13, FastAPI, Uvicorn, Pydantic |
| Deep Learning | PyTorch (ConvLSTM, U-Net, FiLM Conditioning, MC Dropout) |
| Atmospheric & Data | Xarray, Zarr, NumPy, SciPy, H5py, NetCDF4, HTTPX (Open-Meteo NWP) |
| Frontend | React, TypeScript, Vite, MapLibre / Deck.gl, Tailwind CSS |
| Testing & QA | Pytest (352 unit, physics calibration & end-to-end integration tests) |
| Deployment | Localhost (FastAPI on `:8000` + Vite on `:5173`) |

---

## 🏗 Project Architecture

```text
Multi-Modal Inputs (Radar / INSAT / Lightning / ERA5 + Live Open-Meteo 5-Point Stencil)
        │
        ▼
Preprocessing & Grid Alignment (Common Pan-India Grid, QC Masks, Zarr/NetCDF Cubes)
        │
        ▼
APM-Nowcast Neural-Physics Core (ConvLSTM/UNet + FiLM Conditioning + Adaptive Attention + MC Dropout)
        │
        ▼
Mesoscale Post-Processing (Deep-Layer 850/500/300 hPa Steering + 3-Node IDW + Regime Classifier)
        │
        ▼
FastAPI v1 Endpoints (/forecast/latest, /nowcast/snapshot, /scenarios/dropout) ──► React Dashboard
```

---

## 📁 Folder Structure

```text
APM-Nowcast/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── v1/
│   │   │       └── endpoints/       # FastAPI routes (nowcast, health)
│   │   ├── schemas/                 # Pydantic request/response contracts
│   │   ├── services/
│   │   │   ├── inference_service.py # Snapshot builder, regime labeling & dropout fusion
│   │   │   └── live_weather.py      # 5-point stencil, deep-layer steering & IDW engine
│   │   └── main.py                  # FastAPI application entrypoint
│   ├── common/                      # Shared logging & configuration utilities
│   ├── models/                      # PyTorch APMNowcastModel, ConvLSTM, UNet & baselines
│   ├── preprocessing/               # ERA5, INSAT, Radar, Lightning & Zarr grid aligners
│   ├── training/                    # Trainer, checkpoints & multi-metric evaluator (CSI/FSS/ECE)
│   ├── tests/                       # 352 Pytest unit, physics & E2E integration tests
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── api/                     # Typed FastAPI client (nowcastApi.ts)
│   │   ├── components/              # ForecastMap, Navbar, Sensor Dropout Simulator
│   │   ├── types/                   # TypeScript meteorological interfaces
│   │   ├── App.tsx
│   │   └── index.css
│   ├── package.json
│   └── vite.config.ts
├── .gitignore
└── README.md
```

---

## 🔄 Working Process

1. **Atmospheric Ingestion:** Backend fetches live multi-level NWP fields (`CAPE`, `CIN`, `700 hPa RH`, `MSLP`, `10m` gusts, and `850/500/300 hPa` winds) across a 5-point `±1.0°` spatial stencil for strategic Pan-India nodes (or loads aligned Zarr/NetCDF multimodal cubes).
2. **Physics & Steering Computation:** Outer-ring wind vectors cancel internal vortex spin to isolate true storm translation (`motionSpeedKmh`, `bearingDeg`), while moisture/pressure gates suppress false convection in dry capped air.
3. **Adaptive Fusion & Uncertainty Estimation:** Active sensor masks weight Radar, Satellite, Lightning, and NWP attention; missing sensors trigger automatic weight renormalization and epistemic uncertainty inflation.
4. **Spatial Interpolation & Regime Classification:** 3-node IDW interpolation computes continuous local telemetry for every coordinate, assigning physics-backed regime labels (`Cyclonic Cell` vs. `Stable Air Mass`).
5. **Interactive Visualization:** FastAPI serves JSON snapshots to the React frontend for real-time map exploration, lead-time stepping, and sensor-dropout stress testing.

---

## 🧪 Installation

### 1️⃣ Clone this repository
```bash
git clone [https://github.com/suhassrinivasa07/APM-Nowcast.git](https://github.com/suhassrinivasa07/APM-Nowcast.git)
cd APM-Nowcast
```

### 2️⃣ Set up the Python backend environment
```bash
cd backend
python -m venv .venv

# Windows
.venv\Scripts\activate
# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt
cd ..
```

### 3️⃣ Install frontend dependencies
```bash
cd frontend
npm install
cd ..
```

---

## ▶️ Running the Application

### 4️⃣ Start the FastAPI backend (Terminal 1)
```bash
# Activate backend/.venv first, then run from repo root:
uvicorn app.main:app --app-dir backend --reload --port 8000
```

### 5️⃣ Start the React + Vite frontend (Terminal 2)
```bash
cd frontend
npm run dev
```

Then open in your browser:
```text
http://localhost:5173
```

---

## ✅ Testing & Verification

### Run the full backend test suite (352 tests)
```bash
# Windows (cmd)
set PYTHONPATH=.
pytest backend/tests -v

# Linux / macOS / PowerShell
PYTHONPATH=. pytest backend/tests -v
```

### Build the frontend production bundle
```bash
cd frontend
npm run build
```

---

## 📸 Screenshots

> *Add screenshots of your live dashboard (`docs/screenshots/`) showing:*
> - *Pan-India live view with active Bay of Bengal cyclonic core (`SEVERE` / `EXTREME`) and calm inland `Stable Air Mass` cells*
> - *Sensor Dropout Simulator (`Radar Blackout` / `Multi-Feed Failure`) showing attention redistribution and uncertainty inflation*

---

## 🚀 Future Enhancements

- 🛰️ **Live IMD Doppler Radar & MOSDAC INSAT Stream Ingestion** — Connect real-time radar volume scans and satellite brightness temperature feeds alongside Open-Meteo NWP
- 📊 **Automated Benchmark Report Export** — Generate publication-ready Markdown/LaTeX verification tables (`CSI`, `POD`, `FAR`, `HSS`, `FSS`, `Brier`, `ECE`) across sensor-dropout ablations
- ☁️ **Containerized Cloud Deployment** — Package FastAPI + Vite with Docker Compose for cloud GPU/CPU hosting
- 🔔 **Threshold-Based Nowcast Webhooks** — Real-time alert triggers when convective initiation probability or flash density exceeds severe thresholds

---

## 📚 Credits / References

- **Meteorological & NWP Data:** Open-Meteo Atmospheric Forecast API & ERA5 Reanalysis conventions
- **Deep Learning & Verification:** PyTorch, ConvLSTM (Shi et al.), U-Net (Ronneberger et al.), FiLM (Perez et al.), Monte Carlo Dropout (Gal & Ghahramani), and Fractions Skill Score (Roberts & Lean)
- **Geospatial & Web Stack:** FastAPI, React, TypeScript, Vite, and MapLibre GL
