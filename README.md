# Kidney3D-Recon: Automated Segmentation & Volumetric Reconstruction via Bayesian Optimization

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Open3D](https://img.shields.io/badge/Library-Open3D-orange)
![PyMeshLab](https://img.shields.io/badge/Library-PyMeshLab-red)

## 📖 Overview

**Kidney3D-Recon** is an end-to-end pipeline designed for the high-fidelity 3D reconstruction and precise volume estimation of kidneys from CT segmentation masks (NIfTI).

Unlike traditional static reconstruction methods, this project integrates **Bayesian Optimization** to dynamically tune Poisson Surface Reconstruction parameters (`depth` and `scale`) for each individual case. This approach balances geometric accuracy (Chamfer/Hausdorff distance) with volumetric fidelity, achieving medical-grade reconstruction quality.

Key capabilities include automated left/right kidney separation, outlier removal, and density-based mesh post-processing.

---

## 🚀 Key Features

* **Automated Preprocessing**
    * Loads NIfTI (`.nii.gz`) segmentation masks (KiTs23 format).
    * Separates Left/Right kidneys using connected component analysis.
    * Converts voxel masks to point clouds via Marching Cubes.
* **Bayesian Hyperparameter Tuning**
    * Optimizes Poisson reconstruction parameters (`depth` ∈ [7,12], `scale` ∈ [0.8,1.6]).
    * **Loss Function**: Weighted combination of Volume Error (85%) and Geometric Distance (15%).
    * **Fallback Mechanism**: Automatically triggers a "Volume Priority" optimization mode if error exceeds 10%.
* **Advanced Mesh Processing**
    * Statistical outlier removal and normal orientation correction.
    * Density-based face filtering (5th–95th percentile) to remove artifacts.
    * Automated hole closing and non-manifold edge removal using PyMeshLab.
* **Comprehensive Analytics**
    * Automated calculation of Chamfer Distance, Hausdorff Distance, and Volumetric Error.
    * Generation of statistical plots and comparative analysis.

---

## 📊 Performance & Results

Tested on the **KiTs 2023 Challenge Dataset** (50 abdominal CT cases, 96 kidney samples):

| Metric | Statistics |
| :--- | :--- |
| **Segmentation Success** | **96%** (Connected-component separation) |
| **Volume Error** | Mean **2.88%** (Median 1.56%) |
| **Chamfer Distance** | Mean **0.0295** ± 0.0102 |
| **Hausdorff Distance**| Mean **0.1543** ± 0.0716 |
| **Processing Time** | ≈15 min per case (vs. 30 min traditional methods) |

> **Note**: 85.7% of samples achieved a volume error of < 5%.

---

## 🛠️ Installation

### Prerequisites
* **OS**: Ubuntu 20.04 (Recommended) / Windows / macOS
* **Hardware**: NVIDIA GPU recommended for accelerated reconstruction (CUDA 11.8 support).

### Setup

```bash
# 1. Clone the repository
git clone [https://github.com/Chelsea-19/Final-Year-Project.git](https://github.com/Chelsea-19/Final-Year-Project.git)
cd Final-Year-Project

# 2. Create a virtual environment (Recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
# Key libraries: open3d, pymeshlab, bayesian-optimization, nibabel, pandas
pip install -r requirements.txt
```
---

## 📂 Directory Structure
```text
Final-Year-Project/
├── Main_Process/
│   ├── data_utils.py           # Functions for loading and preprocessing point clouds
│   ├── main.py                 # Main script for processing kidneys and generating results
│   ├── metrics.py              # Functions for computing evaluation metrics
│   ├── poisson_reconstruction.py # Functions for Poisson surface reconstruction
│   ├── seg.py                  # Functions for segmentation and volume calculation
│   └── volume_estimation.py    # Function for estimating the volume of a mesh
├── Resluts_Analysis/
│   ├── stats.py                # Script for statistical analysis and visualization of results
│   └── visualization.py        # Script for visualizing segmentation slices
└── README.md                   # This README file
```

---
## 💻 Usage

### 1. Data Preparation
Ensure your dataset follows the KiTs23 structure or place NIfTI files (`.nii` / `.nii.gz`) in a case directory.

### 2. Run the Pipeline
The main script processes cases, generates Ground Truth volumes from masks, and performs reconstruction.

```bash
python Main_Process/main.py
```
Note: Ensure the `base_dir` path in `main.py` points to your dataset location.

### 3. Analyze Results
After processing, use the analysis script to generate statistical charts (saved as `.png` files):
```bash
python Results_Analysis/stats.py
```

## 🤝 Ackownledgement
 - Supervisor: Prof. Fei Ma
 - Dataset: KiTS23 Challenge Organizers
 - Computing Support: Dr. Zixun Lan
 - Open-Source Tools: Open3D, PyMeshLab, BayesianOptimization

---

## 📄 License
This project is licensed under the [MIT License](LICENSE).

---

## ✉️ Contact
If you have any questions or feedback, please feel free to contact the project maintainer at [Felix.Liang24atoutlook.com].

