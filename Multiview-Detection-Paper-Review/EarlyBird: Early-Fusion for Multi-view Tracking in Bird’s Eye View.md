![EarlyBird WACV 2024 Workshop](https://img.shields.io/badge/WACV%20Workshop-2024-f1b800) [Paper](https://arxiv.org/abs/2310.13350)

### Problem Definition

The paper focuses on solving the problem of Multi-Target Multi-Camera (MTMC) tracking using multiple synchronized and calibrated cameras. The main challenges include:

- **Occlusions**: Objects occluded in one camera view might be visible in another, making it necessary to aggregate information across multiple views.
- **Missed Detections**: Caused by occlusions, leading to fragmented tracks and reduced tracking quality.
- **Temporal and Spatial Association**: Current methods typically solve spatial and temporal association separately, introducing errors.

The goal is to develop an **End-to-End** method.

#### End-to-End
- **Definition**: An end-to-end method integrates all components of the task into a single pipeline that can be trained simultaneously using a unified objective function.
- **In EarlyBird**: Detection and tracking are combined into a single architecture, predicting pedestrian locations on the ground plane (Bird’s Eye View, BEV) and learning re-identification (re-ID) features in one step.
- **Benefits**: Simplifies the workflow, ensures joint optimization of model components, and enhances overall performance.

#### Online
- **Definition**: An online method processes video frames sequentially in real-time, without access to future frames.
- **In EarlyBird**: The model predicts pedestrian locations and updates tracklets as it processes each frame.
- **Benefits**: Suitable for real-time applications like video surveillance or autonomous driving.

#### Anchor-free Tracking Method
- **Definition**: Does not rely on predefined bounding boxes but directly predicts object centers or key points.
- **In EarlyBird**: Predicts object locations as occupancy probabilities on the ground plane using heatmaps.
- **Benefits**: Avoids computational overhead and hyperparameter tuning, adaptable to varying object sizes and densities.

The method leverages multi-camera setups with overlapping fields of view, projecting features into a common BEV for detection and tracking.

### Motivation

- **Importance of BEV**: Early-fusion approaches in multi-view detection improve tracking performance by aggregating features into a BEV representation.
- **Limitations of Late-Fusion Approaches**:
  - Detect objects in 2D and associate detections across views, leading to inaccuracies.
  - Require separate optimization for detection and association.
- **Unified Solution**: Combines spatial association (via BEV detection) and temporal association (via re-ID features) for a robust tracking pipeline.

### Methodology

#### 1. Encoder
- Encodes input images using ResNet or Swin Transformer.
- Outputs feature maps downsampled by a factor of 4 while retaining spatial details.

#### 2. Projection
- **Perspective Projection**: Projects 2D image features from each camera into the BEV using a pinhole camera model.
- All features are projected onto the ground plane (z = 0).

#### 3. Aggregation and Decoder
- Aggregates features into a high-dimensional BEV feature map.
- **ResNet-18 Decoder**:
  - Corrects distortions caused by perspective projection.
  - Enhances feature localization and identification using a pyramid architecture.

#### 4. Detection and Losses
- **Center Detection**: Predicts the presence of objects using a heatmap.
- **Offset Prediction**: Refines object locations to reduce quantization errors.
- **Re-ID Features**:
  - Learns identity embeddings for each pedestrian.
  - Combines cross-entropy loss and contrastive loss.

#### 5. Temporal Association
- **Hierarchical Online Association**:
  - Combines Kalman Filters and re-ID features for tracking.
  - Matches objects across frames using appearance (cosine similarity) and motion (Mahalanobis distance).

### Experiments

#### Datasets
1. **Wildtrack**:
   - Real-world dataset with 7 synchronized cameras covering a public area.
   - Includes dense and unscripted pedestrian movements.
2. **MultiviewX**:
   - Synthetic dataset modeled after Wildtrack.
   - Includes 6 virtual cameras and controlled pedestrian trajectories.

#### Metrics
- **Detection Metrics**: MODA (Accuracy), MODP (Precision).
- **Tracking Metrics**: IDF1 (Identity Awareness), MOTA (Tracking Accuracy), MOTP (Multiple Object Tracking Precision).

#### Implementation Details
- **Augmentation**: Random resizing, cropping, and noise injection to improve generalization.
- **Training**:
  - Optimized with Adam.
  - Uses pre-trained weights on ImageNet for both encoder and decoder.
  - Trained for 50 epochs on an RTX 3090 GPU.

#### Datasets
- **Wildtrack**: Real-world dataset with synchronized cameras covering dense pedestrian movements.
- **MultiviewX**: Synthetic dataset with controlled pedestrian trajectories.

#### Metrics
- Detection: MODA (Accuracy), MODP (Precision).
- Tracking: IDF1 (Identity Awareness), MOTA (Tracking Accuracy), MOTP (Precision).

#### Implementation Details
- **Augmentation**: Random resizing, cropping, and noise injection.
- **Training**:
  - Optimized with Adam.
  - Pre-trained on ImageNet.
  - Trained for 50 epochs on an RTX 3090 GPU.

### Results

#### Detection
- Competitive performance on Wildtrack and MultiviewX datasets.
- The decoder improves detection accuracy compared to the baseline (MVDet).

#### Tracking
- Outperforms state-of-the-art methods on Wildtrack:
  - +5.6 IDF1
  - +4.6 MOTA
- Achieves similar improvements on MultiviewX.

#### Ablation Studies
- Components: Augmentation, decoder, and re-ID loss improve performance.
- Encoders: ResNet-18 balances performance and efficiency.

#### Qualitative Results
- Near-perfect tracking in crowded areas.
- Minor errors in border regions with less camera overlap.

### Conclusion

#### Key Contributions
- Early-fusion in BEV improves tracking accuracy.
- Joint detection and re-ID learning enable robust temporal association.

#### Limitations
- Requires synchronized cameras and high-quality annotations.
- Struggles with distortions in border regions with minimal overlap.

### Future Work
- Explore temporal context for detection and tracking.
- Extend to larger datasets and complex scenarios (e.g., traffic surveillance).
- Reduce distortion in BEV projections.
