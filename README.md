# Geochemical-Clustering-PCA-QGIS
PCA + KMeans clustering of geochemical data with spatial visualisation in QGIS.

---

## **README.md (Example for GitHub)**

# 🌍 Geochemical Data Clustering with PCA & QGIS Mapping

## 📌 Overview

This project uses **geochemical survey data** containing elemental concentrations (e.g., Au, Fe, Cu, Zn, Ni, etc.) from different global locations.
We apply **data preprocessing**, **PCA (Principal Component Analysis)** for dimensionality reduction, and **KMeans clustering** to identify geochemical patterns.
Finally, the results are visualised **spatially in QGIS**.

---

## 🗂 Dataset

* **Source:** Geological survey geochemistry dataset (CSV + shapefiles)
* **Columns Used for Analysis:**

  * **Sample\_ID, Latitude, Longitude**
  * Element concentrations in **percent** (e.g., `Al_pct`, `Fe_pct`) and **ppm** (e.g., `Au_ppm`, `Cu_ppm`).
* **Missing Value Handling:**

  * Dropped rows with missing coordinates
  * Filled missing elemental values using the **median** (`SimpleImputer(strategy='median')`)

---

## ⚙️ Methods

### **1. Data Preprocessing**

* Removed unnecessary columns (e.g., administrative details)
* Filled missing values using `SimpleImputer`
* Scaled data with `StandardScaler` to normalize differences between units (%, ppm)

### **2. PCA (Principal Component Analysis)**

* Reduced dimensions from \~20+ elemental variables to **2 principal components (PC1 & PC2)**.
* **Why PCA?**

  * Easier visualisation of high-dimensional data
  * Removes noise and focuses on main patterns

### **3. KMeans Clustering**

* Applied **KMeans (n\_clusters=4)** to group samples into geochemical clusters
* Chosen by **elbow method** and interpretability

---
## **📊 Results & Interpretation**

---


After processing, the dataset was reduced from **N elements** (replace N) to **2 principal components** (PC1 & PC2) using PCA, while retaining **XX% of the variance**.
This means that **XX% of the original information in the dataset is preserved** in our 2D plots.

---

### **1. PCA Variance Retention**

* **PC1** explained **X%** of the variance — strongly influenced by elements like Fe, Mn, and Mg.
* **PC2** explained **Y%** of the variance — more associated with elements like Au, As, and Pb.
* These principal components act as “summary axes” that capture the most important differences between samples.

---

### **2. KMeans Clustering Outcomes**
<img width="839" height="684" alt="Screenshot 2025-08-14 021910" src="https://github.com/user-attachments/assets/804a521f-57be-4400-8e39-ca491df82418" />


<img width="856" height="501" alt="Screenshot 2025-08-14 021938" src="https://github.com/user-attachments/assets/dd8f2e10-c1d4-4e0d-93fc-0be75bbb7de9" />


We selected **K = 4** clusters (based on interpretability).
The clusters group samples with similar geochemical profiles:

| Cluster | Key Characteristics (Higher-than-average elements) | Possible Geological Meaning |
| ------- | -------------------------------------------------- | --------------------------- |
| 0       | High Au, As, Pb, low Fe                            | Potential gold-rich zone    |
| 1       | High Fe, Mn, Mg, low Au                            | Iron-rich lithology         |
| 2       | High Cu, Zn, Ni                                    | Base-metal mineralisation   |
| 3       | Balanced values, moderate Si & Al                  | Background lithology        |

---

### **3. Heatmap Insights**
<img width="1143" height="725" alt="Screenshot 2025-08-14 022006" src="https://github.com/user-attachments/assets/d223c7f4-491f-45b8-805f-93eb7db97b3a" />

The heatmap clearly shows:

* **Cluster 0** stands out for gold (Au) and pathfinder elements like As & Pb.
* **Cluster 1** shows strong iron and manganese concentrations — possibly related to banded iron formations.
* **Cluster 2** has elevated base metals (Cu, Zn, Ni), often linked to sulfide mineralisation.
* **Cluster 3** doesn’t show extreme highs — likely represents typical background rocks.

---

### **4. Spatial Mapping in QGIS**

<img width="1920" height="1080" alt="Screenshot 2025-08-14 020912" src="https://github.com/user-attachments/assets/f4da8f54-77c5-446c-9ee4-3152dbad9461" />


When plotted on a map:

* Clusters are **not randomly distributed** — certain clusters dominate specific geographic areas.
* For example:

  * Cluster 0 samples are concentrated around known gold prospects.
  * Cluster 1 forms continuous zones in the northern part of the study area, possibly linked to specific rock units.
  * Cluster 2 appears scattered near mining activity areas.
  * Cluster 3 is spread widely, representing unmineralised zones.

---

### **5. Implications**

* **Exploration Targeting**: Clusters with high Au & As can be prioritised for gold exploration.
* **Geological Mapping**: Helps distinguish lithological units with similar chemistry.
* **Anomaly Detection**: Isolates unusual samples that don’t match background geochemistry.

---


## 📌 Requirements

* Python 3.9+
* pandas, numpy, scikit-learn, matplotlib, seaborn
* QGIS 3.x for mapping

---

## 📈 Future Work

* Heatmaps for specific elements (e.g., Au concentration)
* Combine with geological layers for interpretation
* Automate map export from Python to QGIS

