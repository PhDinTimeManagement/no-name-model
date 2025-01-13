## Problem Definition
The task addressed by this paper is **multi-view pedestrian detection**, which involves predicting a **Bird’s Eye View (BEV) occupancy map** using multiple synchronised cameras with overlapping fields of view. The challenges include:

1. **Establishing 3D Correspondences**:
• Mapping 2D features from multiple camera views to the 3D ground plane while accounting for occlusions and varying viewpoints.

2. **Aggregating Multi-View Information**:
• Efficiently assembling and combining features across different views to produce a unified representation without losing critical information.

Current methods either:
• Use **full 3D projections**, which are computationally expensive.
• Rely on simple **ground plane projections**, which are prone to misalignments.

The paper proposes a novel **Stacked Homography Transformations (SHOT)** method to approximate 3D projections and effectively aggregate multi-view information for pedestrian detection.

## Motivation
### Limitations of Full 3D Projections:
• While accurate, 3D projections are computationally expensive because they involve high-dimensional grids and 3D convolutions.

### Inaccuracy of Simple 2D Projections:
• Projecting features onto a single 2D plane (ground plane) fails to capture height-dependent features (e.g., head and foot locations), resulting in poor performance in dense scenes.

### Balancing Efficiency and Accuracy:
• The authors aim to develop a method that is computationally efficient while maintaining the accuracy of 3D projections by approximating the process through a **stack of homographies**.

## Methodology
## Experiment
## Result
## Conclusion


