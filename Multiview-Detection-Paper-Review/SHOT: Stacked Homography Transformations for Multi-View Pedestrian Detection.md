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
The **SHOT** framework consists of two main steps:
1. **Construction of Stacked Homographies**.
2. **Soft Selection Module for Feature Aggregation**.

### Stacked Homographies
• **Idea**:
• Instead of projecting features directly onto the ground plane (single 2D homography) or a full 3D grid, SHOT uses a **stack of homographies** to approximate 3D projections.
• Each homography corresponds to a specific height level in the world coordinate system (e.g., ground level, mid-body, head level).

• **Process**:
1. Define a stack of height levels  Z = (formula refer to paper) , where z  is the distance between successive height planes.
2. For each height level, compute a **homography matrix** that maps 2D image coordinates to the corresponding height plane.
3. Project features from the 2D image to all height levels using the computed homographies.

• **Advantages**:
• Efficiently approximates 3D projections with multiple 2D projections.
• Aligns features of different body parts (e.g., feet, torso, head) to the same BEV grid location.

### Soft Selection Module
• **Purpose**:
• Determines which height level (homography) is most relevant for each pixel and aggregates the projected features accordingly.

• **Steps**:
1. Predict a **likelihood map** for each height level, indicating the relevance of that level for every pixel in the feature map.
2. Use a weighted sum to combine the projected features across all height levels, with weights determined by the likelihood map.

• **Key Benefit**:
• Keeps the framework differentiable and end-to-end trainable, allowing the network to learn optimal height-level assignments based on the input data.

### Overall Framework
• **Input**:
• Synchronised images from multiple cameras.

• **Processing Steps**:
1. Extract features from each camera view using a ResNet-18 backbone.
2. Project the features to the BEV grid using SHOT.
3. Concatenate features from all views and refine them with a convolutional network.
4. Predict the final BEV occupancy map (pedestrian locations).

• **Loss Functions**:
• A combination of:
1. **Single-view loss**: Supervises head and foot localization in individual camera views.
2. **Ground plane loss**: Supervises the predicted BEV occupancy map using Gaussian-blurred ground truth annotations.

## Experiment

## Result

## Conclusion



