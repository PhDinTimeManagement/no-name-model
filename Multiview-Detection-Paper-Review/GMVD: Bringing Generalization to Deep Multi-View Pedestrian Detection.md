![GMVD WACV 2023](https://img.shields.io/badge/WACV-2023-f1b800) [Paper](https://arxiv.org/abs/2109.12227)

## Problem Definition
This paper addresses the problem of Multi-View Detection (MVD) for pedestrian detection, focusing on generalization capabilities. While current MVD methods have shown success in occlusion reasoning for crowded environments, they often fail to generalize to:

1. **Varying numbers of cameras**: Adapting to a different number of cameras than the model was trained on.
2. **Different camera configurations**: Changing the spatial arrangement of cameras without retraining.
3. **New scenes**: Applying models trained on one scene (e.g., a traffic signal) to a different scene (e.g., a university campus).

The goal is to develop an MVD model and dataset that can handle these generalization scenarios effectively.

---
## Motivation
### 1. Shortcomings of Current MVD Methods
- Existing state-of-the-art (SOTA) MVD models are trained and tested under fixed conditions (same scene, same camera setup, etc.), leading to poor generalization.
- These models break when the camera configurations or the scene changes, requiring retraining.

### 2. Evaluation Flaws
- Current datasets (e.g., WildTrack and MultiViewX) have overlapping training and test sets, encouraging models to overfit to dataset-specific features rather than generalizing.

### 3. Real-World Deployment Challenges
- Practical scenarios require models to handle

	  - Missing or malfunctioning cameras.
	 
	  - Reconfigurable camera setups.
	 
	  - New and unseen environments.

---
## Methodology
The authors propose a new dataset, **Generalized Multi-View Detection (GMVD)**, and a novel barebones model for MVD that emphasizes generalization.
  
### 1. GMVD Dataset
- **Design**
	  - Generated using synthetic environments (Unity and GTAV) to ensure diversity in lighting, weather, and camera setups.
	  - Covers 7 distinct scenes (6+1) with both indoor (e.g., subway) and outdoor environments.
	  - Includes variations in the number of cameras, their positions, lighting, and weather conditions (sunny, rainy, night, etc.).

- **Train-Test Split**
	  - Ensures no overlap between training and testing to prevent overfitting.
	  - Designed to evaluate generalization across varying conditions.
  
### 2. Proposed Model
- **Feature Extraction and Projection**
	  - Uses ResNet-18 as a backbone to extract features from multiple camera views.
	  - Projects features onto the ground plane (Bird’s Eye View, BEV) using a perspective transformation.

- **Spatial Aggregation**
	  - Employs average pooling for feature aggregation across cameras.
	  - Ensures permutation invariance (camera order does not matter).
	  - Allows the model to handle varying numbers of cameras during inference.

- **Regularization (DropView)**
	  - Randomly drops one camera view during training to simulate real-world scenarios where cameras may malfunction.
	  - Prevents the model from overfitting to specific camera configurations.

- **Loss Function**
	  - Combines KL Divergence and Pearson Cross-Correlation to optimize the predicted occupancy maps against the ground truth.

## Experiments

### 1. Experimental Setup

#### Datasets
- **WildTrack**  
- **MultiViewX**  
- **GMVD (Proposed)**: A diverse, synthetic dataset with 7 scenes, each having unique camera setups and environmental conditions.

#### Metrics
- **MODA (Multiple Object Detection Accuracy)**: Penalizes both false positives and missed detections.
- **MODP (Multiple Object Detection Precision)**: Measures the localization precision of detections.
- **Precision and Recall**: Evaluate the correctness and completeness of detections.

#### Implementation Details
- **Feature Extraction**: ResNet-18 with dilated convolutions for higher spatial resolution.
- **Training**:
  - Optimized using SGD with momentum = 0.9.
  - Early stopping based on validation performance.
- **Inference**:
  - Non-Maximum Suppression (NMS) is applied with a spatial resolution of 0.5m.
  - The model is evaluated with varying numbers of cameras, configurations, and scenes.

---

### Results

#### 1. Generalization to Varying Number of Cameras
- **Setup**:
  - Models trained with 7 cameras on WildTrack were tested with subsets of 4 cameras.
- **Findings**:
  - The proposed model outperformed SOTA methods like MVDet, MVDeTr, and SHOT by a significant margin.
  - DropView regularization further improved performance (MODA = 77.0 vs. 66.6 for SHOT).

#### 2. Generalization to New Camera Configurations
- **Setup**:
  - Models trained on one camera configuration were tested on a different configuration within the same scene.
- **Findings**:
  - SOTA models showed significant performance drops when tested on unseen configurations.
  - The proposed model, especially with DropView, maintained high performance (MODA = 84.0 vs. 75.1 for SHOT).

#### 3. Generalization to New Scenes
- **Setup**:
  - Models trained on MultiViewX (synthetic) were tested on WildTrack (real-world).
- **Findings**:
  - The proposed model achieved a MODA score of 66.1, significantly higher than SHOT (53.6).
  - DropView improved scene generalization by ensuring robustness to unseen environments.
- **Benchmarking on GMVD**:
  - When trained on GMVD and tested on WildTrack, the proposed model achieved a MODA of 80.1, approaching the performance of models trained directly on WildTrack.