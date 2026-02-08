# Predicting Building Types Using OpenStreetMap

**Official implementation of the paper:**

> **Predicting building types using OpenStreetMap**
> , Scientific Reports, Nature Portfolio, 2022


> **Authors:** Kuldip Singh Atwal, Taylor Anderson, Dieter Pfoser, & Andreas Züfle (George Mason University, Emory University)
---

## Abstract

This repository contains the official implementation of the study **“Predicting building types using OpenStreetMap.”** We propose a supervised learning-based approach to fill the gap in descriptive building attributes within OpenStreetMap (OSM). While OSM provides good geometric coverage, semantic information is often sparse. This model leverages geometric properties, topological relationships, and available OSM tags to classify building footprints into **residential** or **non-residential** types without manual intervention.

---

## Key Features

- **OSM-Only Dependency:** The model relies exclusively on OSM features (geometry and tags), eliminating the bottleneck of acquiring region-specific or proprietary datasets.

- **Interpretable Classification:** Uses a C4.5 Decision Tree classifier to ensure model transparency, allowing researchers to understand which features (e.g., building size, proximity to roads) discriminate best between classes.

- **Transfer Learning:** The pre-trained classification models are designed to be transferable; a model trained in one region (e.g., Fairfax County) yields high accuracy when tested in alternative regions (e.g., Mecklenburg County) where ground truth data may be unavailable.

- **Rich Feature Engineering:** Incorporates spatial context by deriving distance to parking lots, proximity to specific road types, and underlying land use polygons to augment sparse tag data.

---

## Methodology

### 1. Data Preprocessing and Extraction

The pipeline utilizes **PyOsmium** and **Geopandas** to extract and process building polygons.

- **Filtering:** Extracts building footprints where the 'building' tag has values, filtering out non-building objects.

- **Meta-categorization:** Maps heterogeneous OSM tags and official ground truth types into binary categories: **Residential** (e.g., apartments, house, dormitory) and **Non-residential** (e.g., commercial, school, industrial, retail).
  
- **Spatial Joins:** Maps OSM buildings to ground truth footprints via spatial intersection to create labeled training sets.

---

### 2. Feature Extraction

The model enhances sparse attributes by deriving new geometric and topological features:

- **Geometric Buffers:** Creates buffers at 30m, 60m, and 90m intervals around specific road categories (e.g., residential vs. non-residential roads) to determine proximity.
- **Parking Proximity:** Calculates the area of nearby parking lots using Jenks natural breaks (bins) and buffers to determine building proximity to parking infrastructure.
- **Tag Encoding:** Utilizes relevant tags—such as `name`, `source`, `addr:street`, `building:levels`, and `amenity`—treated as binary indicator variables.
- **Physical Properties:** Includes building footprint area and intersection with land use polygons.

---

### 3. Model Architecture

The core classification is performed using the **Scikit-Learn Decision Tree Classifier**.

- **Algorithm:** Binary decision tree (C4.5) using the **Gini index** to measure impurity.
- **Pruning:** The tree is pruned when additional criteria increase node impurity by no more than 0.01% to prevent overfitting.
- **Validation:** The model is evaluated using Precision, Recall, and F1-score comparisons against authoritative ground truth data.

---

## Dataset

Study Areas
The model is trained and validated on three distinct study areas representing a mix of urban and suburban environments,:
1. **Fairfax County, Virginia** (Suburban)
2. **Mecklenburg County, North Carolina** (Suburban)
3. **City of Boulder, Colorado** (Urban)
   
Data Sources
• **Input Data:** OpenStreetMap PBF files (volunteered geographic information).
• **Ground Truth**: Official building footprint data obtained from the respective administrative spatial data portals.
• **Availability:** A repository of the processed data used in this study is available at: https://osf.io/3j46v/

---

## Citation

If you use this code, methodology, or data in your research, please cite:

```bibtex
@article{atwal2022predicting,
  title={Predicting building types using OpenStreetMap},
  author={Atwal, Kuldip Singh and Anderson, Taylor and Pfoser, Dieter and Z{\"u}fle, Andreas},
  journal={Scientific Reports},
  volume={12},
  number={1},
  pages={19976},
  year={2022},
  publisher={Nature Publishing Group UK London}
}
```

---

## Acknowledgements

This work is supported by the National Science Foundation Grant No. 2109647 titled “Data-Driven Modeling to Improve Understanding of Human Behavior, Mobility, and Disease Spread”

Computing resources were provided by the Office of Research Computing at George Mason University.
