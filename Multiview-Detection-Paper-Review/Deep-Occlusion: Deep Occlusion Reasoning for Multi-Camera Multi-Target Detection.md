![Deep-Occlusion ICCV 2017](https://img.shields.io/badge/ICCV-2017-f1b800) [Paper](https://openaccess.thecvf.com/content_ICCV_2017/papers/Baque_Deep_Occlusion_Reasoning_ICCV_2017_paper.pdf)

## Problem Definition
The paper addresses the problem of multi-camera, multi-target detection in crowded scenes. Traditional single-camera pedestrian detection methods fail in such scenarios due to:

- **Occlusions**: People occlude each other in crowded environments, leading to missed or inaccurate detections.
- **Crowded Environments**: Single-camera models struggle to handle dense scenes effectively.

The challenge is to design a robust system that:
- Detects multiple people in multi-camera setups.
- Models and reasons about occlusions explicitly.
- Combines deep learning with traditional probabilistic frameworks (e.g., Conditional Random Fields, CRFs).

## Motivation
### Limitations of Single-Image Detectors
- Single-image detectors like Faster R-CNN perform well in simple scenes but degrade significantly in crowded scenes with occlusions.
- Non-Maximum Suppression (NMS) fails to resolve ambiguities caused by overlapping detections.

### Multi-Camera Systems
- Cameras with overlapping fields of view provide an opportunity to overcome occlusions by aggregating information across views.
- Existing multi-camera methods like POM (Probabilistic Occupancy Map) handle occlusions but rely on outdated techniques like background subtraction, which lack discriminative power in dense scenes.
  
### Combining Deep Learning with Probabilistic Models
- Deep learning models can extract powerful features for detection.
- Probabilistic models like CRFs can model interactions (e.g., occlusions) between people and enforce global constraints.

## Methodology
The paper introduces a joint CNN-CRF model to combine the strengths of convolutional neural networks (CNNs) and conditional random fields (CRFs) for multi-camera multi-target detection.
  
### Discretized Ground Plane Representation
- The ground plane is divided into a grid of discrete cells, each representing a potential location for a pedestrian.
- A Boolean variable `Z` is associated with each cell, indicating whether it is occupied.
  
### High-Order CRF
- The CRF models interactions between detections to account for occlusions.

#### Generative Model
- Simulates the expected appearance of the scene (e.g., projections of people onto the ground plane) given their locations.

#### Discriminative Model
- A CNN predicts probabilities of occupancy for each grid cell based on image features.

#### High-Order Potentials
- Measure the agreement between the generative model and the discriminative model using Probability Product Kernels.
- Encode non-local interactions, enabling the system to handle occlusions even when people are far apart.

### Additional CRF Terms
- **Unary Potentials**: Use CNN features to estimate the probability of a pedestrian being present in each grid cell.
- **Pairwise Potentials**: Enforce spatial constraints, ensuring pedestrians are not detected too close to each other.
  
### Inference
- The CRF’s energy function is minimized using Mean-Field Inference, which approximates the posterior distribution over pedestrian locations.

### Training
- **Supervised Training**:
  - The model is trained end-to-end using annotated datasets.
  - Pre-training is used to initialize individual components (e.g., the CNN and generative model) before fine-tuning the entire pipeline.

- **Unsupervised Training**:
  - In scenarios without labeled data, the system uses inter-camera consistency and translation invariance as priors for unsupervised training.

## Experiments
### Datasets
1. **ETHZ**
2. **EPFL Terrace**
3. **PETS 2009**

### Metrics
- **MODA (Multiple Object Detection Accuracy)**: Evaluates overall detection accuracy.
- **MODP (Multiple Object Detection Precision)**: Measures localization precision.
- **Precision and Recall**: Evaluate detection correctness and completeness.

### Baselines
1. **POM-CNN**
   - Combines the traditional POM method with CNN-based segmentation.
2. **RCNN-2D/3D**
   - Projects detections from Faster R-CNN to the ground plane and clusters them across views.
---
## Results
### Quantitative Results
- **On all datasets**, the proposed method outperformed the baselines in terms of MODA, MODP, precision, and recall.
- **ETHZ Dataset**:
  - **Ours**: MODA = 74.1%, MODP = 53.8%, Precision = 95%, Recall = 80%.
  - **RCNN-2D/3D**: MODA = 43%, MODP = 18.4%, Precision = 68%, Recall = 43%.

- **EPFL Dataset**:
  - **Ours**: MODA = 68.2%, Precision = 88%, Recall = 82%.
  - **RCNN-2D/3D**: MODA = 50%, Precision = 39%, Recall = 50%.

### Qualitative Results
- Visualizations show accurate detections in crowded scenes.
- Probabilistic Occupancy Maps provide smooth, interpretable outputs.
  

### Ablation Study
- Removing high-order terms significantly degraded performance, demonstrating their importance for handling occlusions.
- The unsupervised version performed better than baselines but slightly worse than the fully supervised version.

### Comparison with Baselines
- The proposed method consistently outperformed POM-CNN and RCNN-2D/3D baselines across all datasets and metrics.

## Conclusion
### Contributions
- Introduced a novel CNN-CRF pipeline for multi-camera detection with explicit occlusion reasoning.
- Demonstrated the effectiveness of high-order CRF terms for handling crowded scenes.
- Showed the feasibility of unsupervised training for multi-camera systems.

### Advantages
- Does not require Non-Maximum Suppression, avoiding issues with overlapping detections.
- Outputs smooth probability maps for detections, which can be used for further processing (e.g., trajectory tracking).

### Limitations
- The CNN operates independently for each image, rather than pooling multi-camera information early.
- Performance depends on accurate camera calibration and ground plane discretization.

### Future Work
- Investigate multi-camera CNN architectures that pool information early.
- Extend the approach to dynamic scenes and moving cameras.
