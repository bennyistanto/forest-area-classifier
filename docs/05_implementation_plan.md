# 5 Implementation Plan

This chapter outlines the practical steps needed to implement a forest classifier using the data sources (Chapter 2) and the modeling concepts (Chapters 3–4). We will focus on the **2020 reference year** for initial training and validation, then discuss how to extend the classification to other years. A thorough understanding of this chapter will guide the subsequent coding steps.

---

## 5.1 Training Data Creation (2020)

### 5.1.1 Forest vs. Non-Forest Labeling

1. **BNETD Land Cover (2020)** as Ground-Truth  
   - We designate the following classes as “Forest”:
     - Dense Forest (1), Light Forest (2), Forest Gallery (3), Secondary/Degraded Forest (4), Mangrove (5), Forest Plantation/Reforestation (6), Swamp Forest (7), Tree Savannah (16).  
   - All other BNETD classes (8, 9, 10, …, 23) are considered “Non-Forest” for this initial binary classification.

   Formally, for each pixel \(i\), define:

   $$
   y_i = 
   \begin{cases}
   1 & \text{if BNETD class} \in \{1,2,3,4,5,6,7,16\}\\
   0 & \text{otherwise}
   \end{cases}
   $$

   This yields the **reference label** $y_i \in \{0,1\}$.

2. **Optional Refinements**  
   - If needed, we can merge multiple datasets (e.g., we might exclude “Mangrove” from the forest class if the FAO definition requires a particular threshold).  
   - We can also check for **consistency** with other forest cover products (e.g., JRC Forest Cover 2020). However, the primary label source remains BNETD 2020.

### 5.1.2 Feature Engineering for 2020

For each pixel $i$, extract a feature vector $\mathbf{x}_i \in \mathbb{R}^M$. Candidate features include:

1. **Spectral/Vegetation Indices**  
   - **Landsat NDVI (2020)** or **MODIS NDVI (2020)**
     $$
     \text{NDVI}_i = \frac{\mathrm{NIR}_i - \mathrm{RED}_i}{\mathrm{NIR}_i + \mathrm{RED}_i}
     $$
   - Consider **annual mean, min, max, or standard deviation** of NDVI if available.

2. **Canopy Metrics**  
   - **Canopy Cover** (e.g., NASA GFCC 2015, Planet 2019, or JRC 2020).  
   - **Canopy Height** (e.g., Meta/WRI 2020, ETH 2020, or GLAD 2019).

   $$
   x_{i,\text{canopyCover}} = \text{cover\%}_i, 
   \quad
   x_{i,\text{canopyHeight}} = \text{height\_m}_i
   $$

3. **DEM & Terrain**  
   - **Elevation** $(\text{m})$, **Slope** $(^\circ \text{ or } \%)$, **Aspect** $(0-360^\circ)$.  
   - Derived from **Copernicus DEM**.  

4. **Soil/Climate** (Optional)  
   - **Soil Type** or **Soil Moisture** (HiHydroSoil or FLDAS for 2020).  
   - **Annual Precipitation** (CHIRPS 2020), **Temperature** (ERA5-Land 2020).  
   - Helps discriminate moist vs. dry forests.

5. **Distance Features** (Optional)  
   - **Distance to Road** (OSM), **Distance to Water** (DEM).  
   - May capture human pressure or hydrological regime differences.

In practice, we **stack** these layers in a consistent 100m grid. Each pixel is described by a numeric vector:

$$
\mathbf{x}_i = \big[x_{i,\text{NDVI}},\ x_{i,\text{canopyCover}},\ x_{i,\text{slope}},\ x_{i,\text{soil}},\ x_{i,\text{distanceRoad}}, \dots\big].
$$

### 5.1.3 Sample Extraction

Because each image may contain millions of pixels, we often **sample** from the full grid to create a more tractable training set. One approach:

1. **Stratified Sampling**  
   - Sample an equal number of forest and non-forest pixels, or sample proportionally to their actual area.

2. **Spatial Thinning**  
   - Optionally enforce a minimum spacing (e.g., 500 m) between samples to reduce spatial autocorrelation.

3. **Final Training Dataset**  
   $$
   \mathcal{D} = \{(\mathbf{x}_1, y_1), (\mathbf{x}_2, y_2), \dots, (\mathbf{x}_N, y_N)\},
   $$
   where \(N\) might range from thousands to hundreds of thousands of samples, depending on computational capacity.

---

## 5.2 Code Workflow Overview

Below is a modular outline of Python code that orchestrates data loading, feature engineering, model training, and prediction. We will detail each function in the final implementation.

1. **Data Loading and Alignment**
   ```python
   def load_rasters(filepaths_dict):
       """
       Loads raster data from disk (GeoTIFFs), ensures consistent CRS/resolution.
       Returns a dictionary of aligned raster arrays or xarray DataArrays.
       """
       # Implementation details...
       pass
   ```

2. **Feature Extraction**
   ```python
   def extract_features(labels_raster, raster_dict):
       """
       Merges label data (forest vs. non-forest) with feature layers (NDVI, canopy, etc.).
       Possibly returns a pandas DataFrame (rows = samples, columns = feature vars + label).
       """
       # Implementation details...
       pass
   ```

3. **Random Forest Training & Validation**
   ```python
   def train_rf_model(X, y, param_grid=None):
       """
       Trains a Random Forest classifier.
       Optional param_grid for hyperparameter tuning.
       Returns the fitted model and any relevant metrics.
       """
       # Implementation details...
       pass
   ```

4. **Spatial Cross-Validation**
   ```python
   def spatial_cross_val(X, y, coords, param_grid=None, n_folds=5):
       """
       Splits the data into spatial folds, trains RF, validates on hold-out folds.
       Returns cross-validation metrics and best model.
       """
       pass
   ```

5. **Model Prediction**
   ```python
   def predict_map(model, feature_rasters, output_path):
       """
       Applies the trained model to each pixel in feature_rasters, 
       saves a classified map (GeoTIFF) to output_path.
       """
       pass
   ```

6. **Time-Series Extension**
   - Once validated on 2020, the same approach is repeated for each target year (e.g., 1990–2019, 2021–2023), substituting the relevant NDVI, canopy, climate, or soil data for that year.

---

## 5.3 Model Validation & Calibration

### 5.3.1 Confusion Matrix and Accuracy Metrics

For a binary classification of forest vs. non-forest, we compute:

$$
\begin{aligned}
&\text{True Positives (TP)} = \sum_i [\hat{y}_i = 1 \land y_i = 1], \\
&\text{False Positives (FP)} = \sum_i [\hat{y}_i = 1 \land y_i = 0],\\
&\text{False Negatives (FN)} = \sum_i [\hat{y}_i = 0 \land y_i = 1], \\
&\text{True Negatives (TN)} = \sum_i [\hat{y}_i = 0 \land y_i = 0].
\end{aligned}
$$

From these, we derive:

1. **Overall Accuracy**:
   $$
   \text{OA} = \frac{\text{TP} + \text{TN}}{\text{TP} + \text{TN} + \text{FP} + \text{FN}}
   $$

2. **Precision**:
   $$
   \text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}
   $$

3. **Recall**:
   $$
   \text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}
   $$

4. **F1 Score**:
   $$
   F_1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
   $$

### 5.3.2 Spatial k-Fold Cross-Validation

To mitigate overfitting in areas with dense sampling, we propose a **spatial** approach. For instance:

1. Partition the country or study area into $k$ distinct zones (folds).
2. For each fold $j \in \{1,\dots,k\}$:
   - Train on data from all folds except $j$.
   - Test on fold $j$ only.
3. Average metrics across folds for a more robust performance estimate.

Mathematically, if $\text{Acc}_j$ is the accuracy on fold $j$, then the final cross-validation accuracy is:

$$
\overline{\text{Acc}} = \frac{1}{k} \sum_{j=1}^{k} \text{Acc}_j.
$$

### 5.3.3 Probability Calibration (Optional)

Random Forest often provides uncalibrated probabilities $p_i = \hat{P}(y_i=1 \mid \mathbf{x}_i)$. If we need well-calibrated probabilities (e.g., for further modeling or risk analysis), we apply a **calibration mapping** $\phi$:

$$
\hat{p}'_i = \phi\bigl(\hat{p}_i\bigr).
$$

We can fit $\phi$ using **Isotonic Regression** or **Platt Scaling** on out-of-fold predictions.

---

By following these steps, we can develop a robust, reproducible workflow to classify forests vs. non-forests for 2020 and beyond. The next step is to translate this plan into actual **code implementation**, ensuring each function is well-defined and the pipeline is systematically tested with real data.