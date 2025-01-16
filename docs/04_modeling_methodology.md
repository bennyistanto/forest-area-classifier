# 4 Modeling Methodology

This chapter describes the step-by-step approach to building a forest classifier using Earth observation data. We focus on **Random Forest** (RF) as our primary modeling technique, but the underlying concepts—data labeling, feature engineering, cross-validation, and accuracy assessment—can be applied to other machine learning algorithms as well.

---

## 4.1 Data Labeling

In a supervised classification setting, each pixel (or sample) must have a **label** indicating whether it is forest or non-forest (or more detailed classes if doing multi-class classification). Formally, for pixel $i$:

$$
y_i \in \{0, 1\},
$$

where \(y_i = 1\) might represent “forest,” and \(y_i = 0\) might represent “non-forest.”

### Example Data Sources for Labeling
- **Reference Land-Cover Map**: For instance, BNETD Land Cover (2020). If certain classes (e.g., dense forest, light forest, forest gallery) align with a “forest” definition, then those pixels become the positive class $(y_i=1)$, while other classes become the negative class $(y_i=0)$.  
- **Field Surveys**: In some projects, local experts or field plots provide ground-truth data for smaller subsets of pixels.

*(See Chapter 5.1.1 for a more specific example of applying BNETD 2020 to derive forest vs. non-forest labels.)*

---

## 4.2 Feature Extraction

For each labeled pixel $i$, we compile a set of features:

$$
\mathbf{x}_i = \bigl[x_{i,1},\, x_{i,2},\, \dots,\, x_{i,M}\bigr] \in \mathbb{R}^M,
$$

where $M$ is the total number of features. Below are common feature types (which we expand upon in Chapter 5.1.2):

### 4.2.1 Spectral Indices (e.g., NDVI)

A frequently used vegetation index is the **Normalized Difference Vegetation Index** (NDVI). For red (RED) and near-infrared (NIR) reflectances:

$$
\text{NDVI} = \frac{R_{\mathrm{NIR}} - R_{\mathrm{RED}}}{R_{\mathrm{NIR}} + R_{\mathrm{RED}}}.
$$

NDVI often correlates with vegetation density and health, making it useful for distinguishing forest vs. non-forest or different vegetation types.

### 4.2.2 Canopy Cover or Height

If canopy cover or canopy height data are available—e.g., from NASA GFCC, Planet, or GLAD datasets—each pixel can be assigned a numeric value such as:

$$
x_{i,\text{canopy}} = \text{Canopy\_Cover}_i \quad (\text{in \%}), 
$$

or

$$
x_{i,\text{height}} = \text{Canopy\_Height}_i \quad (\text{in meters}).
$$

Areas with higher canopy cover or taller trees generally indicate forest, aligning well with FAO definitions involving minimum height and canopy cover thresholds.

### 4.2.3 Topographic Data

Derived from a Digital Elevation Model (DEM), for example, **Copernicus DEM GLO-30**:

$$
x_{i,\text{elev}} = \text{Elevation}_i,
\quad
x_{i,\text{slope}} = \text{Slope}_i,
\quad
x_{i,\text{aspect}} = \text{Aspect}_i.
$$

These may help differentiate forest types that favor certain altitudes or topographies (e.g., swamp forests in low-lying areas).

### 4.2.4 Soil, Climate & Distance Layers (Optional)

- **Soil Properties** (e.g., soil moisture, organic matter, hydrologic soil group).  
- **Climate Variables** (annual precipitation, temperature) for distinguishing moist vs. dry forest zones.  
- **Distance to Roads/Water** to capture anthropogenic influence or hydrological regimes.

---

## 4.3 Model Setup: Random Forest

A **Random Forest (RF)** is an ensemble of decision trees. Given a training set $\{(\mathbf{x}_i, y_i)\}_{i=1}^N$, RF builds $T$ decision trees $h_t(\mathbf{x})$, each trained on a bootstrap sample of the data and a random subset of features at each split.

### 4.3.1 Decision Tree Construction

Each tree uses a splitting criterion—often the **Gini Impurity** for classification. For a node with samples from $C$ classes (binary classification implies $C=2$), the Gini impurity $G$ is:

$$
G = \sum_{c=1}^{C} p_c (1 - p_c),
$$

where $p_c$ is the fraction of samples belonging to class $c$. The tree algorithm chooses splits that reduce impurity the most.

### 4.3.2 Forest Aggregation

To predict the label $y$ for a new feature vector $\mathbf{x}$, each decision tree $h_t(\mathbf{x})$ outputs a predicted label. The Random Forest then aggregates these “votes” by majority:

$$
\hat{y}(\mathbf{x}) = \text{mode}\bigl\{h_1(\mathbf{x}),\, h_2(\mathbf{x}),\, \dots,\, h_T(\mathbf{x})\bigr\}.
$$

### 4.3.3 Hyperparameter Tuning

Common hyperparameters for Random Forest include:
- $T$: Number of trees in the ensemble
- $\text{max\_depth}$: Maximum depth for each tree
- $\text{max\_features}$: Number (or fraction) of features considered at each split
- $\text{min\_samples\_leaf}$: Minimum samples required in a leaf

We can perform **grid-search** or **random-search** on these hyperparameters to find an optimal combination. In **Chapter 5**, we discuss an example param-grid or Bayesian optimization approach for refining these values under a spatial cross-validation scheme.

---

## 4.4 Validation and Metrics

### 4.4.1 Spatial Cross-Validation

Because pixels can be spatially autocorrelated, we use **spatial cross-validation**. One approach is to partition the study area into $k$ spatial folds, train on $k-1$ folds, and test on the remaining fold. Then rotate through all folds:

$$
\text{CV Score} = \frac{1}{k} \sum_{j=1}^k \text{Accuracy}\bigl(\text{Model}_{-j},\, \text{Fold}_j\bigr),
$$

where \(\text{Model}_{-j}\) is the model trained without fold \(j\), and \(\text{Fold}_j\) is the test fold.

### 4.4.2 Confusion Matrix and Derived Metrics

For a binary classification (forest vs. non-forest), we compute a **confusion matrix**:

$$
\begin{bmatrix}
\text{TP} & \text{FP} \\
\text{FN} & \text{TN}
\end{bmatrix},
$$

where:
- **TP** = True Positives (predicted forest, actually forest)
- **FP** = False Positives (predicted forest, actually non-forest)
- **FN** = False Negatives (predicted non-forest, actually forest)
- **TN** = True Negatives (predicted non-forest, actually non-forest)

From here:

- **Overall Accuracy** $(\text{OA})$:

  $$
  \text{OA} = \frac{\text{TP} + \text{TN}}{\text{TP} + \text{TN} + \text{FP} + \text{FN}}
  $$

- **Precision**:

  $$
  \text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}
  $$

- **Recall**:

  $$
  \text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}
  $$

- **$F_1$ Score**:

  $$
  F_1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
  $$

*(See Chapter 5.3.1 for details on how we’ll compute and interpret these metrics for the 2020 reference data.)*

### 4.4.3 Model Calibration

If we require probabilistic outputs (e.g., the probability a pixel is forest), we may apply calibration to align the raw RF probabilities with observed frequencies. For a set of predicted probabilities $\hat{p}_i$:

$$
\hat{p}'_i = \text{calibration\_model}\bigl(\hat{p}_i\bigr).
$$

Methods like **Platt Scaling** or **Isotonic Regression** can be used based on out-of-fold predictions, ensuring more reliable probability estimates for downstream use (e.g., risk analysis or thresholding).

---

## 4.5 Putting It All Together

1. **Data Labeling**: Identify reference forest vs. non-forest pixels (e.g., from BNETD 2020 classes).  
2. **Feature Extraction**: Compute NDVI, canopy metrics, DEM-based terrain attributes, soil/climate layers, and/or distance features.  
3. **Train**: Fit a Random Forest on labeled samples, using hyperparameter tuning and spatial cross-validation.  
4. **Validation**: Evaluate the model using confusion matrix metrics (Accuracy, Recall, $F_1$, etc.). Optionally apply probability calibration.  
5. **Inference**: Use the final model to classify every pixel (forest vs. non-forest) for each target year or region of interest.  
6. **Post-Processing** (if needed):  
   - Apply smoothing or morphological filters.  
   - Enforce temporal coherence in multi-year classifications.

By following these steps and applying the equations provided, we can develop a robust forest classifier from Earth observation data. In **Chapter 5**, we detail how to translate this methodology into a structured implementation plan: creating training data for 2020, setting up a Python-based workflow, and validating results before extending to other years.