# **Neuroimaging Analysis – Course Repository**

**Authors:**

* **Fatemeh Jafari** – Sessions 1–6
* **Mohammad Saeed Soleimani** – Sessions 7–11

**Program:**
**Neuroscience Research Training Program – fMRI Department, Interdisciplinary School**

This repository contains the complete instructional materials for a hands-on training course in **Neuroimaging Data Analysis using Python**, delivered as part of the neuroscience research training curriculum.

Sessions **1–9** closely follow and implement concepts from the reference textbook:

> **Ariel Rokem & Tal Yarkoni (2024), *Data Science for Neuroimaging: An Introduction*, Princeton University Press**
>

Sessions **10–11** expand into applied fMRI modeling and connectivity analysis using modern neuroimaging tools.

---

# **📁 Repository Structure**

```
session1-review/
session2-numpy/
session3-pandas/
session4-neuroimaging-python/
session5-practical-neuroimaging/
session6-practical-neuroimaging-part 2/
session7-image-processing/
session8-segmentation/
session9-registration/
session10-masking/
session11-firstlevel-GLM/
session11-functional-connectivity/
mini project of 6 first sessions /
project for fMRI analysis
```

---

# **📚 Course Overview**

The course is divided into two major modules:

---

# MODULE 1 — Data Science Foundations for Neuroimaging

### **Sessions 1–6 — Instructor: Fatemeh Jafari**

### **Based on *Data Science for Neuroimaging: An Introduction***



---

## **Session 1 — Python & Data Science Review**

Introduces core Python programming concepts necessary for reproducible neuroimaging analysis.

**Topics**

* Variables, data types, lists, dictionaries
* Loops, conditionals, functions
* Modularity and clean code
* Basic scripting for scientific work

**Outcome:** Students gain functional fluency in Python fundamentals.

---

## **Session 2 — Numerical Computing with NumPy**

Students learn high-performance numerical programming using arrays.

**Topics**

* `ndarray` structure
* Array creation & manipulation
* Broadcasting and vectorized operations
* Statistical operations & mathematical functions

**Outcome:**
Builds the mathematical foundation for MRI/fMRI data (3D/4D arrays).

---

## **Session 3 — Data Manipulation with Pandas**

Essential training for working with experiment metadata, event files, and confounds.

**Topics**

* DataFrame & Series
* Filtering, indexing, grouping
* Joining and merging data tables
* Exploratory data analysis

---

## **Session 4 — Introduction to Neuroimaging in Python**

Students connect Python tools to neuroimaging datasets.

**Topics**

* MRI & fMRI modalities
* NIfTI format and metadata
* Spatial orientation, affine transforms
* Loading images with NiBabel
* Basic visualization (axial/coronal/sagittal)

---

## **Session 5 — Practical Neuroimaging (Part 1)**

Hands-on exploration of real neuroimaging data.

**Topics**

* Navigating 3D/4D NIfTI volumes
* Slice visualization
* Voxel time-series extraction
* Intensity distributions

---

## **Session 6 — Practical Neuroimaging (Part 2 & Mini Project)**

Students combine Python tools to create mini neuroimaging workflows.

**Topics**

* Basic preprocessing
* Exploratory fMRI analysis
* Simple pipeline building
* Data visualization

---

# MODULE 1 → MODULE 2 TRANSITION

### Sessions 7–9 also draw directly from the image processing chapters of

### *Data Science for Neuroimaging* (Ch. 14–16).



---

# MODULE 2 — Core Neuroimaging Analysis

### **Sessions 7–11 — Instructor: Mohammad Saeed Soleimani**

(Expanded detailed explanations included.)

---

# **Session 7 — Image Processing & Multidimensional Neuroimaging Data**

This session builds the conceptual framework to understand MRI/fMRI data as **multidimensional arrays**, an essential perspective for all subsequent analysis.

---

## **1. Images as Numerical Arrays**

Students explore:

* Grayscale → 2D arrays
* RGB → 3D arrays
* MRI → 3D volumes
* fMRI → 4D data (3D + time)

A key demonstration shows how **spatial information is crucial**—shuffling pixel values destroys meaning despite unchanged intensity values.

---

## **2. MRI and fMRI Data Structure**

* Voxel coordinate systems
* Affine matrices linking voxel → world space
* TR and time dimension in fMRI
* Inspection of image header metadata

---

## **3. Practical Work**

* Load Haxby dataset
* Extract slices across different planes
* View single volumes
* Plot voxel-level time-series to understand BOLD fluctuations

---

### **Outcome:**

Students fully understand how neuroimaging data are stored, represented, and manipulated computationally.

---

# **Session 8 — Segmentation**

Students learn how to isolate brain tissue using intensity-based segmentation, as introduced in Ch. 15 of the reference book.


---

## **1. Tissue Intensity Distributions**

Using histograms, students learn to identify:

* Background air
* CSF
* Gray matter
* White matter

---

## **2. Thresholding Approaches**

### **Mean Thresholding**

* Fast but imprecise
* Good for rough background removal

### **Otsu's Method (Single Threshold)**

* Maximizes inter-class variance
* Automatically finds optimal threshold

### **Multi-Otsu Segmentation**

* Produces multiple classes (background, GM, WM, high-CSF)
* Suitable for rough volumetric tissue separation

Students compare the masks visually and evaluate segmentation quality.

---

## **3. Visualization & Evaluation**

* Mask overlays
* Binary vs multi-class segmentation
* Tissue boundaries

---

### **Outcome:**

Students gain intuition for tissue intensity behavior and practical segmentation workflows.

---

# **Session 9 — Registration**

Based on Ch. 16 of *Data Science for Neuroimaging*.


Registration aligns images into a shared spatial frame—an essential step for fMRI preprocessing, subject alignment, and multimodal integration.

---

## **1. Why Registration Is Required**

* Align anatomical and functional images
* Correct for movement
* Standardize subjects into template/MNI space
* Compare across sessions and individuals

---

## **2. Transformations**

Students develop deep understanding of:

* Translation
* Rotation
* Scaling
* Shearing
* Affine matrices

RGB overlay visualizations show misalignment clearly.

---

## **3. Cost Functions & Optimization**

* **MSE** for same-modality alignment
* **Mutual Information (MI)** for multi-modality alignment
* Gradient-based optimization considerations

---

## **4. Multiresolution Strategy**

Students implement a pyramid approach:

1. Coarse alignment (downsampled)
2. Medium refinement
3. Fine full-resolution adjustment

This prevents optimization from falling into poor local minima.

---

## **5. Practical DIPY Implementation**

* Set up `AffineRegistration`
* Define metric (MI), interpolator, optimizer
* Estimate transform
* Apply transform to moving image
* Visual inspection before/after

---

### **Outcome:**

Students become capable of performing and evaluating affine registration on neuroimaging datasets.

---

# **Session 10 — Masking, Brain Atlases & Maskers**

A full introduction to ROI-based neuroimaging workflows.

---

## **1. Masks**

Students learn to create and use masks to:

* Remove non-brain regions
* Define anatomical structures
* Constrain GLM/modeling to desired voxels

---

## **2. Atlases**

Using the Harvard–Oxford atlas, students:

* Load atlas images
* Inspect region labels
* Extract ROI masks
* Visualize ROI boundaries

---

## **3. Maskers in Nilearn**

Maskers convert neuroimaging data → 2D matrices usable in machine learning and statistics.

Students learn four major maskers:

### **NiftiMasker**

Voxel-level extraction with:

* Standardization
* Confound removal
* Filtering & smoothing

### **NiftiLabelsMasker**

ROI-summary signals based on atlas labels.

### **NiftiMapsMasker**

Probabilistic atlas support (e.g., ICA maps).

### **NiftiSpheresMasker**

Seed-based extraction for connectivity.

---

### **Outcome:**

Students master ROI creation, atlas navigation, and time-series extraction pipelines.

---

# **Session 11a — First-Level GLM Analysis**

Students learn how to build and fit statistical models to fMRI time-series.

---

## **1. The GLM Framework**

At each voxel:

[
Y = X\beta + \epsilon
]

Where:

* **Y**: BOLD time series
* **X**: Design matrix (HRF-convolved regressors + confounds)
* **β**: Parameter estimates
* **ε**: Noise (autocorrelated)

---

## **2. Design Matrix Construction**

Students build:

* Task regressors (onset/duration/type)
* Convolution with HRF models
* High-pass filters
* Motion regressors
* Physiological confound regressors
* Scrubbing regressors

Design matrix quality checks include collinearity and shape diagnosis.

---

## **3. Model Fitting**

Using `FirstLevelModel`, students:

* Fit voxelwise regression
* Estimate β maps
* Create contrasts
* Generate t- and z-statistics

---

## **4. Thresholding & Visualization**

* Voxelwise thresholds
* FDR & FWE corrections
* Cluster-level inference
* Interpretation cautions

---

### **Outcome:**

Students gain full proficiency in first-level fMRI statistical modeling and interpretation.

---

# **Session 11b — Functional Connectivity**

Students move into network-based neuroimaging analysis.

---

## **1. Conceptual Introduction**

Functional connectivity reflects **temporal co-fluctuation** between brain regions.
Students learn distinctions between:

* Functional connectivity
* Effective connectivity
* Structural connectivity

---

## **2. Fisher Z-Transform**

Used to normalize correlation values:

[
Z = \tanh^{-1}(r)
]

Enables proper group averaging and statistical tests.

---

## **3. Complete Connectivity Pipeline**

### **1 — Preprocessing**

Motion correction, confound regression, filtering, normalization.

### **2 — ROI Definition**

Atlases (Harvard–Oxford, AAL, Schaefer) or seed coordinates.

### **3 — Time-Series Extraction**

Using maskers to produce:

* ROI × time matrices
* Seed time-series

### **4 — Connectivity Computation**

* Correlation matrices
* Seed-to-voxel maps
* Z-transform operations

### **5 — Visualization**

Heatmaps, connectome plots, thresholded maps.

---

## **4. Advanced Methods**

### **Beta-Series Connectivity (BSC)**

Trial-wise GLM regressors → beta estimates → ROI correlations.

### **Psychophysiological Interaction (PPI)**

Tests task-modulated connectivity by adding an interaction term to the GLM.

---

### **Outcome:**

Students learn modern methods to analyze and interpret functional brain networks.

---

# **🎯 Final Learning Outcomes**

After completing this course, students will be able to:

### **Programming & Data Handling**

* Use Python, NumPy, Pandas for scientific computing
* Manipulate multidimensional MRI/fMRI datasets
* Write reproducible, modular analysis code

### **Neuroimaging Analysis**

* Load, visualize, and inspect NIfTI files
* Perform segmentation and registration
* Build atlas-based ROI pipelines
* Extract voxel- and ROI-level time series

### **Statistical & Network Analysis**

* Build and evaluate first-level GLM models
* Compute and interpret functional connectivity
* Implement advanced methods (BSC, PPI)
* Interpret results responsibly

---

# **📘 Reference**

All concepts in Sessions **1–9** are based on:

**Ariel Rokem & Tal Yarkoni (2024)**
***Data Science for Neuroimaging: An Introduction***
Princeton University Press


---

# **🤝 Acknowledgments**

This course is part of the **Neuroscience Research Training Program**,
**fMRI Department – Interdisciplinary School**.

---



### **Course Developers**

* **Fatemeh Jafari** — Sessions 1–6 ( foundational sessions on data science & neuroimaging)
* **Mohammad Saeed Soleimani** — Sessions 7–11  (advanced neuroimaging analysis modules)


