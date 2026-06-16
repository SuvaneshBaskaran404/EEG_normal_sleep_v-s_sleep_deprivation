# EEG_normal_sleep_v-s_sleep_deprivation
# Electrophysiological Dynamics of Acute Sleep Deprivation

This repository contains the code and analytical pipeline for classifying acute sleep deprivation versus normal sleep states using high-density, resting-state Electroencephalography (EEG) data. The project utilizes a combination of advanced signal processing, manual feature extraction, and machine learning/deep learning architectures (including EEGNet, CNNs, and Random Forests) to identify neurological biomarkers of fatigue.

📊 Dataset Information

This project is built upon the open-access dataset published by Xiang et al. (2024). The dataset consists of eyes-open resting-state EEG recordings from 71 healthy participants, recorded across two distinct sessions: a baseline following Normal Sleep (NS) and a session following 24 hours of Sleep Deprivation (SD).

Dataset Name: A resting-state EEG dataset for sleep deprivation (Accession Number: ds004902)

Official Dataset Link: OpenNeuro ds004902

Official Research Paper: Scientific Data (Nature) - https://doi.org/10.1038/s41597-024-03268-2

📁 Repository Structure

The repository is organized into the following directories and files.

Data Folders

ds004902_data/
This is the core raw data directory structured in the standard BIDS (Brain Imaging Data Structure) format.

sub-01/ to sub-71/: Individual participant folders. Each contains a ses-1 (Normal Sleep) and ses-2 (Sleep Deprivation) subfolder with the raw EEG recordings saved in EEGLAB format (.set and .fdt).

participants.tsv & participants.json: Metadata files detailing participant demographics (age, sex) and behavioral scores (like the PSQI sleep quality index).

dataset_description.json & README: Standard BIDS metadata describing the overall experiment.

task-eyesopen_events.json: Describes the event markers and triggers recorded during the resting-state paradigm.

Processed_data/
A directory designated for storing intermediate outputs, such as serialized NumPy arrays (.npy) of extracted features or epoched data. This prevents the need to re-run the computationally expensive feature extraction loops from scratch.

Code & Notebooks

Processing_3.ipynb
The primary data engineering and traditional Machine Learning notebook. This file handles:

Loading raw .set files using MNE-Python.

Applying bandpass filters (0.5–40 Hz / 0.5-20 Hz) to remove DC drift and high-frequency noise.

Slicing the continuous signal into 5-second overlapping epochs.

Feature Extraction: Condensing raw time-series data into 854 targeted features per epoch, including Hjorth parameters, Spectral Entropy, and specific frequency band powers (Delta, Theta, Alpha, Beta).

Machine Learning Models: Pipelines for training and evaluating tabular models such as Random Forest, XGBoost, and Logistic Regression with intra-subject normalization.

EEGNET.ipynb & EEGNET (2).ipynb
The Deep Learning pipelines of the project. These notebooks take the epoched arrays and feed them directly into Neural Networks.

Contains the implementation of EEGNet, a specialized compact convolutional neural network designed specifically for brain-computer interfaces.

Contains implementations of 1D and 2D Convolutional Neural Networks (CNNs).

Implements rigorous evaluation strategies, including standard 80/20 train-test splits and Leave-One-Subject-Out (LOSO) cross-validation to account for inter-subject variability.

Documentation

ParentResearchPaper.pdf
A local copy of the original Xiang et al. (2024) publication (A resting-state EEG dataset for sleep deprivation). Kept for offline reference regarding spatial/spectral findings and the experimental protocol.

⚙️ Methodology Highlights

Addressing Inter-Subject Variability: Due to massive differences in baseline skull thickness and natural brain rhythms across the 71 subjects, standard global normalization fails. This project implements Intra-Subject Z-score Normalization to track relative brainwave changes rather than absolute microvolt values.

Train/Test Splitting: Employs a strict subject-wise split (e.g., training on Subjects 1-56, testing on Subjects 57-71) to ensure zero data leakage and to validate that the models are finding universal markers of fatigue, not memorizing specific participants.

🚀 Getting Started

To replicate this analysis:

Clone the repository.

Download the raw dataset from OpenNeuro (ds004902) and place it inside the ds004902_data/ directory.

Install the required dependencies: mne, scipy, scikit-learn, xgboost, tensorflow.

Run Processing_3.ipynb to explore the feature extraction and classical ML approach, or run EEGNET (2).ipynb for the deep learning pipeline.
