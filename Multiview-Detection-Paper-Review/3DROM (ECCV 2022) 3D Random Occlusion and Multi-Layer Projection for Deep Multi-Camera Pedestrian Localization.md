![3DROM ECCV 2022](https://img.shields.io/badge/ECCV-2022-f1b800) [Paper](https://arxiv.org/abs/2207.10895)

Problem Definition
The paper addresses the challenge of multi-view pedestrian detection under heavy occlusions. Single-view deep-learning methods often fail in such scenarios due to:

Occlusions: Severe occlusion among pedestrians in crowded scenes leads to missed detections.
Data Limitations: A scarcity of annotated multi-view datasets increases overfitting risks in deep-learning models.
The aim is to develop a robust multi-view detection framework that:

Detects pedestrians accurately even in heavily occluded settings.
Reduces overfitting despite limited annotated multi-view training samples.
Effectively fuses features across multiple views and pedestrian heights.
Motivation

Limitations of Existing Methods:

Single-view models struggle with occlusions, resulting in degraded performance.
Current multi-view approaches, though effective, lack sufficient training data for robust performance.
Importance of Geometric Constraints:

Incorporating 3D geometric constraints helps align features across camera views for accurate detection.
Need for Advanced Augmentation:

Existing monocular augmentations like random erasing violate multi-view geometric consistency, making them unsuitable for this task.
Methodology
The proposed method, 3D Random Occlusion and Multi-Layer Projection (3DROM), builds upon the MVDet framework and introduces:

3D Random Occlusion:

Simulates pedestrian occlusions by placing 3D cylinders on the ground plane and back-projecting them to all camera views during training.
Multi-Layer Projection:

Projects view-specific feature maps onto multiple parallel planes at different heights (e.g., ground level, torso, head) to capture full pedestrian silhouettes across views.
Feature Alignment:

Uses deformable convolution (DCNv2) to correct geometric distortions in multi-layer projections.
Loss Function:

Combines top-view occupancy map loss and single-view detection loss for heads and feet.
Experiments
The model was evaluated on three public datasets: WILDTRACK, MultiviewX, and EPFL Terrace, with metrics like MODA, MODP, precision, and recall. Key findings include:

Significant performance improvements over state-of-the-art methods like SHOT and MVDet.
Ablation studies showed the effectiveness of 3D Random Occlusion and multi-layer projection.
Five-layer projections below waist height yielded the best results.
Results

Performance: The 3DROM model achieved top MODA scores (e.g., 95.0% on MultiviewX).
Ablation Studies: Both components (3D occlusion and multi-layer projection) improved detection, particularly in dense and occluded settings.
Robustness: Increased detection precision and recall across datasets compared to existing methods.
Conclusion
The 3DROM framework enhances multi-view pedestrian detection by addressing overfitting and occlusion challenges. Future work aims to improve generalizability across datasets and develop more efficient feature fusion strategies.
