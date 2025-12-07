
# **Inter Disciplinary School**
# **Neuroimaging Analysis – Course Repository**

**Authors:**

* **Fatemeh Jafari** – Sessions 1–6
* **Mohammad Saeed Soleimani** – Sessions 7–11

This repository contains all teaching materials for a complete, hands-on course in **Neuroimaging Data Analysis using Python**.
The course gradually builds students’ skills—from foundational Python and data science (Sessions 1–6) to full neuroimaging workflows including segmentation, registration, GLM modeling, masking, ROI analysis, and functional connectivity (Sessions 7–11).

Sessions 1–6 follow the textbook ***Data Science for Neuroimaging***.
Sessions 7–11 are expanded practical modules developed specifically for this course.

---

# **📁 Repository Structure**

```
session1-review/
session2-numpy/
session3-pandas/
session4-neuroimaging-python/
session5-practical-neuroimaging/
session6-project/

session7-image-processing/
session8-segmentation/
session9-registration/
session10-masking/
session11-firstlevel-GLM/
session11-functional-connectivity/
```

---

# **📚 Course Overview**

The course is divided into two modules:

* **Module 1** — Data Science Foundations (Sessions 1–6)
  *Instructor: Fatemeh Jafari*

* **Module 2** — Neuroimaging Core Analysis (Sessions 7–11)
  *Instructor: Mohammad Saeed Soleimani*

---

# **MODULE 1 — Data-Science Foundations for Neuroimaging**

### **Sessions 1–6 — Instructor: Fatemeh Jafari**

*Based on “Data Science for Neuroimaging”*

---

## **Session 1 — Python & Data Science Review**

**Topics Covered**

* Basic data types and variables
* Control flow: loops, conditionals
* Functions and modularity
* Lists, dictionaries, tuples
* Good scientific coding practices

**Outcome:**
Students become comfortable with essential Python skills used heavily in numerical and neuroimaging analysis.

---

## **Session 2 — Numerical Computing with NumPy**

**Topics**

* Understanding the `ndarray` data structure
* Creating arrays: `zeros`, `ones`, random arrays, identity matrices
* Slicing, indexing, reshaping
* Broadcasting rules
* Vectorized math operations
* Reductions (mean, sum, std, argmax, etc.)

**Outcome:**
Students gain the ability to manipulate multidimensional numerical data efficiently—core for 3D MRI volumes and 4D fMRI data.

---

## **Session 3 — Data Manipulation with Pandas**

**Topics**

* DataFrame and Series
* Input/output (CSV, TSV, Excel)
* Selecting columns/rows
* Filtering & boolean indexing
* Groupby operations
* Merging and joining tables

**Outcome:**
Students learn how to manage event files, confounds, behavioral metadata, and statistical tables for future GLM and connectivity analysis.

---

## **Session 4 — Introduction to Neuroimaging in Python**

**Topics**

* Basics of MRI, fMRI, and neuroimaging modalities
* NIfTI format and metadata
* Understanding spatial axes, affine matrices, voxel sizes
* Using NiBabel to load images
* Visualizing slices

**Outcome:**
Students understand how neuroimaging data are stored, structured, and visualized using Python libraries.

---

## **Session 5 — Practical Neuroimaging (Part 1)**

**Topics**

* Working with real neuroimaging datasets
* Loading 3D/4D images
* Extracting single slices
* Time-series visualization of voxels
* Exploring intensity distributions

**Outcome:**
Students learn the first practical steps in interacting with real MRI and fMRI datasets.

---

## **Session 6 — Practical Neuroimaging (Part 2 & Mini Project)**

**Topics**

* Building analysis scripts for fMRI
* Combining Python + NumPy + Pandas + neuroimaging tools
* Creating simple preprocessing and visualization workflows
* Completing a guided analysis project

**Outcome:**
Students finish a mini-project and demonstrate readiness for advanced neuroimaging tasks.

---

# ---

# **MODULE 2 — Neuroimaging Core Analysis (Sessions 7–11)**

### **Instructor: Mohammad Saeed Soleimani**

*Expanded detailed explanations included.*

---

# **Session 7 — Image Processing & Multidimensional Neuroimaging Data**

This session introduces how images—including neuroimaging volumes—are represented as **multidimensional arrays**, and how this conceptual shift empowers efficient analysis.

---

### **1. Images as Arrays**

* **2D images (grayscale)** → matrices of shape `(H × W)`
* **RGB images** → `(H × W × 3)`
* Demonstration: shuffling pixels destroys structure but not pixel values, highlighting the importance of spatial order.

---

### **2. Transition to MRI/fMRI Data**

* **MRI** = 3D data → `(X, Y, Z)`
* **fMRI** = 4D data → `(X, Y, Z, T)` representing time
* Each element in these arrays = **voxel** (3D equivalent of a pixel)

Students inspect:

* Voxel coordinates
* Affine transformations (mapping voxel → world coordinates)
* TR (repetition time)
* Time-series for specific voxels (to understand BOLD fluctuations)

---

### **3. Practical Demonstrations**

* Loading Haxby dataset
* Viewing anatomical & functional slices
* Extracting volumes at different time points
* Plotting voxel time-series

---

### **Learning Outcome**

Students deeply understand the structure of neuroimaging data and how to manipulate/visualize it efficiently.

---

# **Session 8 — Segmentation for Neuroimaging**

Segmentation separates the brain from non-brain structures and differentiates tissues like **gray matter (GM)**, **white matter (WM)**, and **CSF**.

---

## **1. Understanding Intensity Histograms**

Students learn how different tissues produce different peaks in intensity histograms:

* Background air → lowest intensities
* CSF → low–medium
* Gray matter → medium
* White matter → higher intensities

Histograms guide thresholding decisions.

---

## **2. Thresholding Methods**

### **Mean-based Threshold**

* Simple and fast
* Suitable for background removal
* Poor for separating tissues with overlapping intensities

### **Otsu’s Method (1-threshold)**

* Automatic threshold selection
* Maximizes separation between foreground/background
* Better than simple mean thresholding

### **Multi-Otsu (multi-class segmentation)**

* Produces multiple thresholds → multiple tissue classes
* Useful for:

  * Separating CSF / GM / WM
  * Rough skull stripping

Students compare segmentation masks to the original image and evaluate performance.

---

## **3. Visualizing Segmentation Results**

* Overlaying masks on images
* Color-coded tissue labels
* Extracting segmented brain volumes

---

## **Learning Outcome**

Students learn practical segmentation workflows, understand their limitations, and gain intuition about MRI tissue intensities.

---

# **Session 9 — Image Registration**

Registration aligns images so that corresponding anatomical structures overlap correctly.

---

## **1. Why Registration Is Necessary**

* Align functional images to anatomical images
* Correct for head motion
* Align images from different sessions/sources
* Prepare images for group analysis (MNI normalization)

---

## **2. Transformations**

Students learn:

* **Translation** (shifting)
* **Rotation**
* **Scaling**
* **Shearing**
* **Affine transformation** = combination of all above

Visual examples show how images misalign and how channels (red/green overlays) highlight misalignment.

---

## **3. Cost Functions**

Registration depends on optimizing similarity metrics:

* **Mean Squared Error (MSE)** – good for same-modality images
* **Mutual Information (MI)** – robust for multi-modality (T1 vs T2, or anatomical vs functional)

Students learn why MI is preferred for MRI-based registration.

---

## **4. Multiresolution Strategy**

A pyramid approach:

1. Downsample & blur image → coarse alignment (global structure)
2. Medium resolution → refine
3. Full resolution → fine adjustment

This avoids local minima and stabilizes optimization.

---

## **5. Implementation with DIPY**

Students implement:

* `AffineRegistration`
* Choosing metrics, interpolators
* Running registration step-by-step
* Inspecting the estimated affine matrix
* Visual comparison before/after registration

---

## **Learning Outcome**

Students acquire practical skills for performing and evaluating affine registration on MRI images.

---

# **Session 10 — Masking, Brain Atlases & Maskers**

Masking defines which parts of the brain to analyze. Atlases define anatomical or functional regions.

---

## **1. Masking Concepts**

* **Binary masks** select voxels for analysis
* Masks help:

  * Remove non-brain regions
  * Focus analysis on ROIs
  * Improve statistical power by reducing noise

Students create masks manually and explore automated ones.

---

## **2. Atlases**

Students work with:

* **Harvard–Oxford cortical atlas**
* Label maps where each voxel has a region ID
* Region names mapped to index values

Tasks include:

* Listing atlas labels
* Extracting single-region masks
* Plotting ROI overlays

---

## **3. Maskers (Nilearn)**

Maskers convert brain images → numerical matrices.

Students learn four key maskers:

### **`NiftiMasker`**

* Extracts voxel time-series from a binary mask
* Performs:

  * Standardization
  * Detrending
  * Smoothing
  * Confound removal

### **`NiftiLabelsMasker`**

* ROI-level signals based on atlas labels

### **`NiftiMapsMasker`**

* For probabilistic atlases (e.g., ICA networks)

### **`NiftiSpheresMasker`**

* For seed-based analyses

---

## **Learning Outcome**

Students learn how atlas-based ROIs are created and used, and how maskers simplify extraction of meaningful brain signals.

---

# **Session 11a — First-Level GLM (Voxelwise fMRI Modeling)**

Students learn the complete pipeline for modeling task-based fMRI responses.

---

## **1. The GLM Framework**

At each voxel, we model:

[
Y = X\beta + \epsilon
]

Where:

* **Y** = voxel time-series
* **X** = design matrix (task regressors + confounds + drifts)
* **β** = estimated response amplitudes
* **ε** = noise

Students learn:

* Why fMRI noise is autocorrelated
* HRF convolution
* How regressors represent task events

---

## **2. Building the Design Matrix**

Includes:

* Task conditions (convolved with **HRF**)
* High-pass filters
* Motion parameters
* Scrubbing regressors
* Physiological confounds (WM/CSF signals)

Students visualize the design matrix and check for:

* Collinearity
* Correct timing
* Regressor strength

---

## **3. Model Fitting in Nilearn**

Using `FirstLevelModel`, students learn:

* Fitting voxelwise GLMs
* Creating contrasts (simple, differential, custom vectors)
* Generating:

  * **beta maps**
  * **t-statistics**
  * **z-statistics**

---

## **4. Statistical Thresholding**

Students explore:

* Voxelwise uncorrected thresholds
* **FDR** correction
* **Cluster-level correction**
* Interpreting statistical maps carefully

---

## **Learning Outcome**

Students develop full competence in building and evaluating first-level GLM models for task-based fMRI data.

---

# **Session 11b — Functional Connectivity Analysis**

Students transition from activation-based analysis to **network-based** analysis.

---

## **1. What Is Connectivity?**

Connectivity reflects **statistical synchrony** of brain regions over time.
Students understand differences:

* **Functional Connectivity (FC)** → correlation
* **Effective Connectivity** → causal influence
* **Structural Connectivity** → white-matter pathways

---

## **2. The Fisher Z-transform**

Because correlations are bounded and non-normal:
[
Z = \tanh^{-1}(r)
]

Used for:

* Averaging
* Group-level statistics
* Hypothesis testing

---

## **3. Complete Connectivity Pipeline**

### **Step 1 — Preprocessing**

Includes motion correction, slice-time correction, normalization, and confound regression.

### **Step 2 — Defining ROIs**

* Atlases (Harvard–Oxford, AAL, Schaefer)
* Spherical seeds around coordinates
* Networks from ICA maps

### **Step 3 — Extracting Time Series**

Using maskers:

* `NiftiLabelsMasker` for ROI–ROI
* `NiftiSpheresMasker` for seed-to-voxel

### **Step 4 — Computing Connectivity**

Students compute:

* ROI–ROI correlation matrices
* Seed-to-voxel correlation maps
* Group-averaged connectivity matrices

### **Step 5 — Visualizing Connectivity**

* Heatmaps
* Connectome diagrams
* Thresholded correlation matrices

---

## **4. Advanced Methods**

### **Beta-Series Connectivity (BSC)**

* GLM with one regressor per trial
* Trial-wise betas correlated across ROIs
* Captures trial-to-trial interplay

### **Psychophysiological Interaction (PPI)**

* Interaction of:

  * Task model
  * Seed time-series
  * Interaction terms
* Measures task-modulated connectivity

---

## **Learning Outcome**

Students can construct ROI and seed-based connectivity analyses and understand advanced task-modulated connectivity models like PPI and BSC.

---

# ---

# **🎯 Final Learning Outcomes for the Entire Course**

Students completing the course will be able to:

### **Programming & Data Handling**

* Write clean Python code for scientific computing
* Use NumPy and Pandas to manipulate massive datasets
* Work fluently with NIfTI neuroimaging files

### **Neuroimaging Foundations**

* Understand 3D/4D brain image structure
* Visualize slices, volumes, and voxel time-series
* Apply segmentation and registration techniques

### **Advanced Analysis**

* Build and evaluate GLM models
* Generate contrast maps and interpret statistics
* Construct ROI–ROI and seed-based connectivity analysis
* Use maskers and atlases for data extraction and ROI creation

---

# **🤝 Acknowledgments**

This course builds on:

* **Data Science for Neuroimaging**
* The Python neuroimaging ecosystem (NiBabel, Nilearn, DIPY, scikit-image)

### **Course Developers**

* **Fatemeh Jafari** — Sessions 1–6
* **Mohammad Saeed Soleimani** — Sessions 7–11


