# Geochemical-Clustering-PCA-QGIS
PCA + KMeans clustering of geochemical data with spatial visualisation in QGIS.

---


# 🌍 Geochemical Data Clustering with PCA & QGIS Mapping 

## 📌 Overview

This project uses **geochemical survey data** containing elemental concentrations (e.g., Au, Fe, Cu, Zn, Ni, etc.) from different global locations.
We apply **data preprocessing**, **PCA (Principal Component Analysis)** for dimensionality reduction, and **KMeans clustering** to identify geochemical patterns.
Finally, the results are visualised **spatially in QGIS**.

---

## 🗂 Dataset

**Title**: *Global Geochemical Database for Critical Minerals in Archived Mine Samples* (USGS)  
**Citation**: Granitto, M. _et al._, 2020. *Global Geochemical Database for Critical Minerals in Archived Mine Samples: U.S. Geological Survey data release*. DOI: [10.5066/P9Z3XL6D](https://doi.org/10.5066/P9Z3XL6D).

**Summary**:  
Contains geochemical and geological information for historic ore and ore-related rock samples from both the U.S. and 27 additional countries across major continents. Originating from the USGS “Quick Assessment of Rare and Critical Metals in Ore Deposits” project (2008–2013), the dataset supports critical mineral potential assessments and ore system modeling.

| Attribute          | Details                                         |
|-------------------|--------------------------------------------------|
| **Timeframe**     | Sample collection: June 11 2011 – August 1 2017; Publication: June 23 2020 |
| **Formats**       | `.csv`, `.xlsx`, `.accdb`, metadata `.xml`/`.txt`, data dictionary `.csv`/`.xlsx`, shapefiles, and ArcGIS map service definitions |
| **Geographic Scope** | U.S. (multiple states) & 27 countries including Argentina, Australia, Brazil, China, India, Norway, Peru, South Africa, Sweden, Zambia, etc. |
| **Purpose**       | To assess previously mined ore deposits for critical minerals and support mineral resource evaluation efforts. |

You can explore and access the dataset via the USGS ScienceBase portal.


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


After processing, the dataset was reduced from **N elements**  to **2 principal components** (PC1 & PC2) using PCA, while retaining **55% of the variance**.
This means that **55% of the original information in the dataset is preserved** in our 2D plots.

---

### **1. PCA Variance Retention**

* **PC1** explained **35.2%** of the variance — strongly influenced by elements like Fe, Mn, and Mg.
* **PC2** explained **20.1%** of the variance — more associated with elements like Au, As, and Pb.
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


## 🗺 Geospatial Visualization & QGIS Integration

<img width="1062" height="439" alt="Screenshot 2025-08-14 031537" src="https://github.com/user-attachments/assets/69f173a9-aa1a-48f4-9953-bcac4f89ea43" />


After performing **PCA** and **K-Means clustering** on the geochemical dataset, results were mapped to geographic coordinates (Latitude, Longitude) using **GeoPandas** and **Shapely**.

The notebook generates:

* **Clustered scatter plot** (PCA space)
* **Geospatial cluster map** (real-world locations)
* **Exported files** for QGIS:

  * `geochem_clusters.geojson` (GeoJSON format)
  * `geochem_clusters.shp` (ESRI Shapefile)

These files can be directly imported into **QGIS** for further analysis, styling, and overlay with geological layers.

**Code snippet for export:**

```python
gdf = gpd.GeoDataFrame(
    df_pca_full,
    geometry=[Point(xy) for xy in zip(df_pca_full['Longitude'], df_pca_full['Latitude'])],
    crs="EPSG:4326"
)
gdf.to_file("geochem_clusters.geojson", driver="GeoJSON")
gdf.to_file("geochem_clusters.shp", driver="ESRI Shapefile")
```

**Example Output in QGIS:**
Clusters are displayed with unique colors, highlighting anomalous zones potentially linked to mineralization.


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

  ---
  Referecences
* Sadeghi, M., … (2024). Principal components analysis and K-means clustering of till geochemical data…

* Jansson, N.F., … (2022). Principal component analysis and K-means clustering as tools during exploration for Zn-skarn deposits…

* Hajihosseinlou, M., Maghsoudi, A., & Ghezelbash, R. (2024). Geochemical anomaly detection and pattern recognition: A combined study of the Apriori algorithm, PCA and spectral clustering.

