Replication Package: Behavioral Transparency in Multi-Agent RL

Submission: Nature Machine Intelligence

Authors: Kenny Ching / University of Auckland

License: MIT

This repository contains the code and data required to replicate the findings, figures, and statistical analyses presented in the manuscript "Behavioral Transparency in Multi-Agent RL: Strategic Decoupling and the Economic Optimization of Tactical Shocks".
📂 Repository Structure

    code/: Jupyter notebooks for data acquisition, processing, and econometrics.

    data/processed/: The final unified event-level dataset (tf_events_master_unified.csv) used for the stratified regression analysis.

    data/raw/: Destination folder for raw JSON replays (not included due to size).

🚀 Quick Replication (Results Only)

To reproduce the Figures and Tables from the paper without re-downloading the raw match data:

    Clone this repository.

    Install dependencies: pip install -r requirements.txt

    Open code/FINAL_OpenAI_DOTA_analysis.ipynb.

    Run all cells. The notebook will load the provided tf_events_master_unified.csv and generate Tables 1–2 and Figures 1–5.

🛠 Full Replication (From Scratch)

If you wish to rebuild the dataset from the source API, follow this pipeline:

    Data Acquisition:

        Run code/FINAL_OpenAI_DOTA_download.ipynb.

        Note: This script downloads the raw match files (~5GB). It requires an active internet connection and may take several hours due to API rate limits.

    Data Processing:

        Run code/FINAL_OpenAI_DOTA_file_prep.ipynb.

        This parses the JSON files, classifies positive and negative tactical shocks, and calculates the Strategic Yield (ΔGt​) across multiple temporal horizons (t∈{30,60,120,180} seconds) strictly following the conclusion of the combat event. It outputs tf_events_master_unified.csv.

    Analysis:

        Run code/FINAL_OpenAI_DOTA_analysis.ipynb.

        This applies the Nearest Neighbors (k=5) matching algorithm to control for environmental geometries, and reproduces the full Econometric Convergence Map, Conversion Efficiency metrics, and density distributions.

📊 Data Availability Statement

The raw match replay files used in this study are the property of Valve Corporation. Due to repository size limits, the raw JSON files are not hosted here. However, they are publicly accessible via the OpenDota API. The FINAL_OpenAI_DOTA_download.ipynb script included in this repository automates the retrieval of the exact match cohort used in the study.

The processed dataset (tf_events_master_unified.csv) is included in this repository and is sufficient for all statistical replication.
