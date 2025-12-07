# Neuroimaging Analysis – Course Materials

This repository contains teaching materials for a practical course in neuroimaging
analysis with Python. The current version focuses on core topics in **fMRI and MRI
image processing**, including:

- Image representation and basic processing
- Image segmentation
- Image registration
- Masking, ROI definition, and brain atlases
- First-level GLM analysis
- Functional connectivity analysis

All examples are implemented in Python using scientific and neuroimaging libraries
such as **NumPy, Matplotlib, scikit-image, Nilearn, NiBabel, DIPY**, and related tools. 

---

## Repository Structure

> Adjust filenames/paths below to match how you organize your repo (e.g. `notebooks/`,
> `slides/`, etc.).

- `S7_image_processing.*` – Image Processing & Multidimensional Images  
- `S8_Segmentation.*` – Image Segmentation for Neuroimaging  
- `S9_Registration.*` – Image Registration (Alignment)  
- `S10_Masking.*` – Masks, Atlases, and Maskers  
- `S11_GLM_FirstLevel.*` – First-Level Analysis & General Linear Model  
- `S11_FunctionalConnectivity.*` – Functional Connectivity Analysis  

Each session combines **conceptual explanations** with **hands-on code** that students
can run and modify.

---

## Prerequisites

Students are expected to have:

- Basic Python programming experience (variables, functions, loops).
- Some familiarity with linear algebra and statistics (vectors, matrices, correlations,
  regression/GLM).
- Introductory knowledge of MRI/fMRI (what BOLD signal is, basic experimental design).

Recommended environment:

- Python 3.x
- `numpy`, `scipy`, `matplotlib`
- `scikit-image`
- `nibabel`
- `nilearn`
- `dipy`
- `pandas`

---

## Session-by-Session Overview

### Session 7 – Image Processing for Neuroimaging

**Goal:** Understand images as numerical arrays and extend this view to multi-dimensional
neuroimaging data (3D volumes, 4D fMRI). :contentReference[oaicite:1]{index=1}  

**What we cover:**

- **Images as arrays**
  - A grayscale image as a 2D array `(height × width)`.
  - Color images as 3D arrays `(height × width × channels)`, with RGB channels.
  - Why spatial relationships matter: scrambling pixels destroys structure even though
    the numeric values are unchanged (demonstrated with a shuffled photograph).
- **Basic operations on images**
  - Viewing images with Matplotlib.
  - Simple intensity operations such as inversion (`255 - image`) and their effects.
- **Extending to higher dimensions**
  - 3D volumetric data: structural MRI as arrays `(Z, Y, X)` and the concept of **voxels**
    (volume pixels).
  - 4D data: fMRI as `(X, Y, Z, T)` – a time series of 3D volumes.
  - Example using the Haxby dataset:
    - Loading a 4D BOLD image.
    - Inspecting voxel sizes and TR.
    - Extracting single volumes and plotting them.
    - Extracting and plotting a voxel time series to visualize the temporal dimension.

**Skills gained:**

- Thinking of images (2D, 3D, and 4D) as NumPy arrays.
- Plotting slices and volumes.
- Interpreting voxel time series in fMRI data.

---

### Session 8 – Image Segmentation

**Goal:** Learn how to separate brain tissue from background and other structures using
intensity-based methods, and understand their limitations. :contentReference[oaicite:2]{index=2}  

**What we cover:**

- **Why segmentation?**
  - Remove background air and non-brain voxels.
  - Avoid including skull and CSF when we want only brain tissue.
  - Move toward more specific ROIs (e.g., gray vs white matter).

- **Intensity histograms**
  - Plotting voxel intensity histograms for a brain slice.
  - Interpreting separate peaks for background, brain tissue, skull, and CSF.

- **Simple thresholding**
  - Using the mean intensity as a crude threshold.
  - Building a binary mask where voxels above the threshold are set to 1.
  - Overlaying the segmentation mask on the original slice.

- **Otsu’s method**
  - Concept: choose the threshold that minimizes intra-class variance (or maximizes
    inter-class variance).
  - Manual loop implementation to build intuition.
  - Using `skimage.filters.threshold_otsu` for efficient computation.
  - Comparing the mean-based vs Otsu’s threshold visually.

- **Multi-Otsu segmentation**
  - Extending to **multiple classes** (e.g., background, brain tissue, bright CSF/skull).
  - Using `threshold_multiotsu` to obtain several thresholds.
  - Converting the slice into labeled regions via `np.digitize`.

**Skills gained:**

- Creating basic segmentation masks from intensity thresholds.
- Understanding when simple thresholding is sufficient (e.g., removing air) and when
  more advanced methods are needed.
- Interpreting segmentation outputs and their failure cases (e.g., CSF misclassified as
  brain).

---

### Session 9 – Image Registration

**Goal:** Understand and implement image registration to align images (2D or 3D) so that
corresponding anatomical structures overlap. :contentReference[oaicite:3]{index=3}  

**What we cover:**

- **Conceptual foundations**
  - Registration: aligning two images (fixed and moving) so that the same anatomy is in
    the same place.
  - Everyday motivations illustrated with photographs: small shifts, rotations, and zoom
    differences.
  - Importance in neuroimaging:
    - Aligning functional images to anatomical images.
    - Correcting head motion across runs or sessions.

- **Affine and similarity transformations**
  - Transform components: translation, rotation, scaling, shearing.
  - Example: using a known similarity transform to artificially misalign an image
    (astronaut example).
  - Visual “stereo overlay” where red and green channels show misalignment.

- **Optimization for registration**
  - Defining a **cost function** (e.g., Mean Squared Error, Mutual Information).
  - Iteratively adjusting transform parameters to minimize the cost.
  - Motivation for multi-resolution (coarse-to-fine) strategies to avoid local minima.

- **Multilevel / multiresolution registration**
  - Building Gaussian pyramids (blur + downsample).
  - Parameters:
    - `factors` – downsampling factors per level.
    - `sigmas` – smoothing at each level.
    - `level_iters` – maximum iterations per level.
  - Intuition: coarse levels capture large global misalignments; finer levels refine
    details.

- **Practical implementation with DIPY**
  - Using `AffineRegistration` and `MutualInformationMetric`.
  - Setting multiresolution schedules (`factors`, `sigmas`, `level_iters`).
  - Using `AffineTransform2D` to recover unknown transformation between fixed and
    moving images.
  - Comparing overlay **before** and **after** registration and inspecting the
    estimated affine matrix.

**Skills gained:**

- Conceptual understanding of registration and why it is essential in MRI/fMRI workflows.
- Practical experience with affine registration in Python using DIPY.
- Interpreting optimization logs and evaluating registration quality.

---

### Session 10 – Masking, Atlases, and Maskers

**Goal:** Learn how to use masks and atlases to define regions of interest (ROIs) and how
to extract brain signals using Nilearn’s masker objects. :contentReference[oaicite:4]{index=4}  

**What we cover:**

- **Masks**
  - Definition: binary images (same size as the brain image) with 1 = include, 0 =
    ignore.
  - Examples:
    - Whole-brain mask that excludes skull and air.
    - ROI mask targeting a specific structure (e.g., hippocampus).
  - Intuition: masks act like cookie cutters for brain images.

- **Atlases**
  - Definition: labeled brain maps where each voxel has an integer ID corresponding
    to a region name.
  - Example: Harvard–Oxford cortical atlas.
  - Understanding background vs labeled regions and mapping IDs to region names.

- **Working with a cortical atlas (Harvard–Oxford)**
  - Fetching the atlas with `nilearn.datasets.fetch_atlas_harvard_oxford`.
  - Loading the atlas image with NiBabel and inspecting its shape and affine transform.
  - Listing unique integer labels present in the volume and matching them to region
    names.
  - Visualizing atlas overlays with `plot_roi` and contour views.

- **Creating ROI masks from an atlas**
  - Selecting a specific region ID and building a binary mask from the labeled image.
  - Building a `Nifti1Image` for the region mask.
  - Plotting the ROI mask on a standard MNI template.

- **Maskers in Nilearn**
  - Concept: objects that convert NIfTI images into numeric matrices while handling
    masking, resampling, standardization, confound regression, filtering, and smoothing.
  - Overview of key maskers:
    - `NiftiMasker` – voxel-level extraction inside a binary mask.
    - `NiftiLabelsMasker` – ROI-level extraction using a labels/parcellation image.
    - `NiftiMapsMasker` – ROI-level extraction using probabilistic maps.
    - `NiftiSpheresMasker` – ROI extraction based on spherical seeds.

**Skills gained:**

- Using atlases to define anatomical ROIs.
- Creating binary masks for selected regions.
- Understanding when and how to use Nilearn’s different masker classes in analysis
  pipelines.

---

### Session 11a – First-Level GLM and Statistical Modeling

**Goal:** Learn how to model task-based fMRI data using the **General Linear Model (GLM)**
at the single-subject (first-level) and understand its extension to group analyses
(second-level). :contentReference[oaicite:5]{index=5}  

**What we cover:**

- **The GLM for fMRI**
  - Voxelwise regression model:  
    \( Y = X\beta + \varepsilon \), where:
    - \(Y\): BOLD time series.
    - \(X\): design matrix with task regressors + confounds + drift terms.
    - \(\beta\): regression coefficients (betas).
    - \(\varepsilon\): residual noise (AR(1) modeled in Nilearn).
  - Role of **contrasts** for hypothesis testing (e.g., A > B).

- **First-level vs second-level**
  - First-level: per-subject, per-run modeling to obtain beta and contrast maps.
  - Second-level: group-level GLM on contrast maps across subjects to make population
    inferences.

- **Design matrix construction**
  - Building task regressors from events (`onset`, `duration`, `trial_type`) and
    convolving with an HRF (e.g., SPM HRF).
  - Adding drift regressors (cosine basis) for high-pass filtering.
  - Visual inspection of the design matrix to verify correctness.

- **Confound modeling**
  - Including motion parameters, white-matter/CSF signals, scrubbing regressors, etc.
  - Passing confounds separately to `FirstLevelModel.fit` to avoid mixing them with
    task regressors.
  - Emphasis on how proper confound handling improves sensitivity and reduces bias.

- **Running the GLM in Nilearn**
  - Creating and fitting a `FirstLevelModel` with appropriate TR, HRF model, and noise
    model.
  - Defining contrasts by name or explicit contrast vectors.
  - Computing contrast maps (beta, t, or z) using `compute_contrast`.
  - Thresholding statistical maps with `threshold_stats_img` (FDR/FWE/cluster).
  - Visualizing unthresholded and thresholded maps with `plot_stat_map`.

**Skills gained:**

- Building design matrices for fMRI experiments.
- Fitting first-level GLMs and computing contrasts in Nilearn.
- Understanding how second-level GLM extends these ideas to group analysis.

---

### Session 11b – Functional Connectivity

**Goal:** Understand functional connectivity (FC), its relationship to activation analysis,
and how to compute connectivity measures with Nilearn. :contentReference[oaicite:6]{index=6}  

**What we cover:**

- **Definition and scope**
  - FC as statistical dependence (co-fluctuation) between brain regions over time.
  - Distinctions:
    - FC ≠ structural connectivity.
    - FC ≠ effective connectivity.
    - FC is commonly measured via correlations.

- **Fisher Z-transform**
  - Problem with correlation coefficients being bounded and non-normally distributed.
  - Using `np.arctanh(r)` to transform to Fisher-Z space for averaging and statistics.
  - Using `np.tanh(Z)` to convert back to correlations for interpretation.

- **Connectivity analysis pipeline**
  1. **Preprocessing & confounds**
     - Slice timing correction, motion correction, normalization.
     - Confound regression (motion, WM/CSF, scrubbing, etc.).
  2. **ROI/seed definition**
     - Atlas-based ROIs (e.g., Harvard–Oxford, Schaefer, AAL).
     - Seed regions defined from coordinates or activation peaks.
  3. **Time-series extraction**
     - Using `NiftiLabelsMasker` (atlas) or `NiftiSpheresMasker` (seeds).
     - Standardization and detrending of time series.
  4. **Connectivity computation**
     - ROI–ROI correlation matrices using `ConnectivityMeasure`.
     - Seed-to-voxel maps.
     - Use of Fisher Z for group statistics.
  5. **Thresholding and inference**
     - Uncorrected thresholds, FDR, cluster, or FWE correction.
  6. **Atlas-guided interpretation**
     - Overlaying thresholded seed maps with an atlas.
     - Summarizing which regions show significant connectivity.

- **Advanced task-based connectivity**
  - **Beta-series connectivity (BSC):**
    - GLM with one regressor per trial, then correlations of trial-wise betas across ROIs.
  - **Psychophysiological interaction (PPI):**
    - GLM with psychological (task), physiological (seed), and interaction regressors
      to test task-modulated connectivity.

- **Interpretation guidelines**
  - Distinguishing activation (GLM contrasts) from connectivity (correlations).
  - Being transparent about preprocessing, thresholding, and atlas choice.
  - Avoiding causal claims and over-interpretation, especially for negative correlations.

**Skills gained:**

- Implementing ROI–ROI and seed-to-voxel connectivity analyses.
- Using Fisher Z-transforms for group comparison of connectivity.
- Understanding BSC and PPI as advanced task-based connectivity methods.
- Critically interpreting connectivity results.

---

