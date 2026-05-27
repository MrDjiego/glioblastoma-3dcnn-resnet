# glioblastoma-3dcnn-resnet

# Glioblastoma MGMT Methylation Classifier | 3D CNN ResNet

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00.svg?logo=tensorflow)](https://www.tensorflow.org/)
[![Clinical AI](https://img.shields.io/badge/Domain-Clinical_AI-00C853.svg)]()

## Executive Summary
A 3D Convolutional Neural Network (CNN) featuring residual connections designed to predict the MGMT promoter methylation status in glioblastoma patients using Magnetic Resonance Imaging (MRI). 

**Clinical Impact:** Glioblastoma is highly aggressive. Determining MGMT methylation status is critical for tailoring chemotherapy treatments. This deep learning pipeline aims to classify this status non-invasively directly from MRI scans, potentially saving critical diagnostic time and bypassing the risks and inconvenience associated with surgical brain biopsies.

## Data Engineering & Domain Logic
The model was trained using the **RSNA-ASNR-MICCAI** dataset. 

A critical data engineering decision was to exclusively utilize **FLAIR (Fluid-Attenuated Inversion Recovery)** MRI sequences. By applying a long Inversion Time (TI), the FLAIR sequence effectively nullifies the signal from fluids with long T1 relaxation times (like cerebrospinal fluid). This suppresses anatomical noise, preventing the CSF from causing interference, while pathological processes (which increase tissue water content) are highlighted as hyperintense regions.

* **Volumetric Standardization:** All scans were standardized to >32 slices, feeding the network with a uniform `(128, 128, 64, 1)` tensor shape.

## Architecture & Hardware Optimization
The architecture is a custom-built 3D CNN inspired by ResNet. 
* **Residual Blocks:** Implements custom skip connections (`x = x + x1`) across 3D convolutional layers to mitigate the vanishing gradient problem in deep volumetric processing.
* **Memory Management:** Due to strict hardware constraints and RAM limitations during development, the network and data loaders were heavily optimized to process high-dimensional 3D spatial data efficiently without memory overflows.

## Performance Metrics
The model underwent rigorous 4-fold cross-validation to ensure generalization and avoid overfitting. 
* **Mean Performance:** Accuracy: `0.742` | AUROC: `0.750`
* **Peak Fold Performance:** Accuracy: `0.909` | AUROC: `0.960`

While these baseline metrics show promising capability for non-invasive classification, further scaling of the dataset and resolution (beyond current hardware limits) will bridge the gap to full clinical-grade deployment.

## Local Execution

**1. Clone the repository and install dependencies:**
```bash
git clone [https://github.com/yourusername/glioblastoma-3dcnn-resnet.git](https://github.com/yourusername/glioblastoma-3dcnn-resnet.git)
cd glioblastoma-3dcnn-resnet
pip install -r requirements.txt
```

2. Data Placement:
Ensure your preprocessed .npy volumes are placed in the data/ directory.

3. Train the Model:
Executes the training loop and saves the best .h5 artifact to the models/ directory.

Bash
python train.py

4. Evaluate & Generate Artifacts:
Loads the compiled weights, generates predictions, and outputs the Confusion Matrix and ROC Curve visuals.

Bash
python evaluate.py

***
