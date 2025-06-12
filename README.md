# Paris Métro Degradation Analysis & GNN Modeling
Developed By: Suhail Akhtar, Shubham Baishthakur, Dr Nur MM Kalimullah
Trinity College Dublin, Ireland
> **Note**: This repository contains only the Jupyter notebooks used for exploratory data analysis and modeling, plus the trained GNN model files. All other code (data‐processing scripts, full source modules, raw CSVs, large HTML maps) have been excluded.  

---

## 🚆 Project Overview

This project analyzes degradation scenarios on the Paris Métro network, reroutes passenger flows when specific links (track segments) are taken out of service, and trains a Graph Neural Network (GNN) to predict post‐failure segment loads. The key steps are:

1. **Build the Métro line‐level graph** from interstation connectivity and GPS CSVs.  
2. **Generate bypassable degradation scenarios** (edge outages or station closures).  
3. **Assign OD (origin–destination) passenger flows** on the intact network and on each degraded network, computing new link loads, travel‐time increases, and required headways.  
4. **Visualize** the changes on interactive Folium maps (baseline vs. degraded).  
5. **Train & tune** a GNN (using PyTorch Geometric) to predict segment‐level passenger loads under arbitrary degradation.

This repo includes only:

- **Notebooks**: Step‐by‐step analysis of data processing, scenario generation, rerouting, and GNN training.  
- **Models**: Pre‐trained GNN weights that you can load for inference or further fine‐tuning.  

---

## 📂 Repository Structure

- **`notebooks/`**  
  - **MAIN.ipynb**  
    *Loads the raw CSVs (`gps_locations.csv`, `interstation_connectivity.csv`, `passenger_flow.csv`), builds the line‐level multigraph (`NetworkX MultiDiGraph`), and computes baseline passenger flows and link loads.*  
    *Generates all bypassable edge‐failure and station‐closure scenarios, ranks the top‐N critical edge outages by peak baseline load, reassigns flows after each outage, and visualizes each scenario (Folium heatmaps + Excel exports).*
    *Converts rerouted NetworkX graphs into PyTorch Geometric `Data` objects, defines the GNN architecture (`GraphConv` layers + edge‐MLP), splits into train/validation/test, uses Optuna to search for best hyperparameters, trains the final model on combined data, and evaluates performance.*  

- **`models/`**  
  - **`gnn_optuna.pt`**  
    *The final GNN trained on all scenarios (after Optuna tuning), ready for inference.*  

> **Note**: The notebooks assume that the raw CSV files (`gps_locations.csv`, `interstation_connectivity.csv`, `passenger_flow.csv`) live in a directory called `data/raw/` at the same level as this repository. They also expect a folder `data/processed/` where the pickled line‐graph (`paris_metro_linegraph.gpickle`) and other intermediate files are saved. If you only have this reduced repo, see the **Setup** section below for guidance.

---

## ⚙️ Setup & Dependencies

> **Warning**: The notebooks reference data files and scripts that are not included here. These instructions tell you how to recreate the necessary graph and data for notebooks to run from scratch.  

1. **Clone this repository**  
   ```bash
   git clone git@github.com:findmussa/paris-metro-analysis.git
   cd paris-metro-analysis# paris-metro-analysis
