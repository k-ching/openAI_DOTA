# Replication Package: Emergent Risk Asymmetry in AI Self-Play

**Submission:** Nature Machine Intelligence  
**Authors:** Kenny Ching/University of Auckland 
**License:** MIT  

This repository contains the code and data required to replicate the findings, figures, and statistical analyses presented in the manuscript *"Emergent risk asymmetry in high-dimensional multi-agent self-play"*.

## 📂 Repository Structure

* `code/`: Jupyter notebooks for data acquisition, processing, and econometrics.
* `data/processed/`: The final event-level dataset (`tf_events_final-1.csv`) used for the regression analysis ($N=34,799$).
* `data/raw/`: Destination folder for raw JSON replays (not included due to size).

## 🚀 Quick Replication (Results Only)

To reproduce the Figures and Tables from the paper without re-downloading the raw match data:

1.  Clone this repository.
2.  Install dependencies: `pip install -r requirements.txt`
3.  Open `code/03_statistical_analysis.ipynb`.
4.  Run all cells. The notebook will load the provided `tf_events_final.csv` and generate Figures 1–4.

## 🛠 Full Replication (From Scratch)

If you wish to rebuild the dataset from the source API (OpenDota), follow this pipeline:

1.  **Data Acquisition:**
    * Run `code/01_data_download.ipynb`.
    * *Note:* This script downloads ~2,300 match files (~5GB). It requires an active internet connection and may take several hours due to API rate limits.
    
2.  **Data Processing:**
    * Run `code/02_data_processing.ipynb`.
    * This parses the JSON files, calculates the "Gold Advantage Delta" ($\Delta G_{180s}$), and outputs `tf_events_final.csv`.

3.  **Analysis:**
    * Run `code/03_statistical_analysis.ipynb`.

## 📊 Data Availability Statement

The raw match replay files used in this study are the property of Valve Corporation. Due to repository size limits, the raw JSON files are not hosted here. However, they are publicly accessible via the OpenDota API. The `01_data_download.ipynb` script included in this repository automates the retrieval of the exact match cohort used in the study.

The **processed dataset** (`tf_events_final.csv`) is included in this repository and is sufficient for all statistical replication.
