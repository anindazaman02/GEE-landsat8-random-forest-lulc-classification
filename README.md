# Supervised LULC Classification using Landsat 8 OLI Architecture (2015)

[![Google Earth Engine](https://img.shields.io/badge/Google_Earth_Engine-JavaScript-blue?logo=googleearthengine&logoColor=white)](https://earthengine.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository contains an automated Google Earth Engine (GEE) JavaScript script designed to perform Land Use/Land Cover (LULC) supervised classification for the year 2015. The pipeline utilizes surface reflectance data from the Landsat 8 Operational Land Imager (OLI) sensor, training a Random Forest classifier to isolate urban footprints and export the classified raster for spatial modeling.

---

## 🛰️ Technical Implementation & Parameters

The processing script executes the following key methodological steps:

### 1. Image Collection & Preprocessing
* **Sensor Target:** Landsat 8 Surface Reflectance Tier 1 Level 2 (`LANDSAT/LC08/C02/T1_L2`).
* **Temporal Window:** January 10, 2015, to March 25, 2015 (optimized for cloud-free winter scenes).
* **Quality Control:** Filtered for scenes containing `< 2%` cloud cover.
* **Reduction & Clipping:** Combined into a singular image using a `.mean()` pixel reducer and tightly clipped to the defined Area of Interest (`aoi`).

### 2. Spectral Predictor Bands
The machine learning model utilizes six core reflective bands of the Landsat 8 OLI sensor to assess surface characteristics:
* `SR_B2` (Blue), `SR_B3` (Green), `SR_B4` (Red), `SR_B5` (Near-Infrared), `SR_B6` (Shortwave Infrared 1), and `SR_B7` (Shortwave Infrared 2).

### 3. Classifier Configuration & Training
* **Algorithm:** Smile Random Forest (`ee.Classifier.smileRandomForest`) running **100 decision trees**.
* **Target Schema:** Evaluates three primary landcover categories compiled from user training geometries: `water`, `vegetation`, and `urban`.

### 4. Accuracy Assessment
* **Data Partitioning:** Implements an advanced **70/30 randomized splitting ratio** ($70\%$ data allocated for model calibration, $30\%$ completely isolated for validation).
* **Validation Output:** Computes and prints a **Confusion Matrix**, **Overall Accuracy**, and the **Kappa Coefficient** to the console to measure classification performance.

---

## 💻 How to Run This Script

### Prerequisites
1. An active [Google Earth Engine](https://earthengine.google.com/) account.
2. A polygon asset drawing labeled as `aoi` indicating your boundary.
3. Pre-defined training geometries mapped on your canvas canvas labeled as: `water`, `vegetation`, and `urban`.

### Instructions
1. Open the [GEE Online Code Editor](https://code.earthengine.google.com/).
2. Copy the contents of the script file from this repository and paste it into your workspace canvas.
3. Click **Run**. The map will automatically center on your AOI, render the True Color raster, display the classified output, and populate validation data in the right-hand Console.

---

## 📦 Data Export Tasks
The script contains a live, un-commented task pipeline configured to export data directly to your Google Drive:
* **Product:** Classified Landuse Raster (`GeoTIFF`).
* **Target Folder:** `GEE_Exports`
* **Resolution:** 30 meters.
