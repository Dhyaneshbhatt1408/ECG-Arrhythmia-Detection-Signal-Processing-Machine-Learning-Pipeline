# ECG Arrhythmia Detection Pipeline

**Resume bullet:** Built an end-to-end ECG arrhythmia detection pipeline in Python, 
applying biomedical signal processing (bandpass filtering, beat segmentation) to real 
PhysioNet clinical data and training a Random Forest classifier evaluated on clinically 
meaningful per-class sensitivity, with patient-level data splitting to prevent leakage.

## What This Project Does

This notebook builds a biomedical signal processing pipeline that detects and classifies 
cardiac arrhythmias from raw ECG signals, using the MIT-BIH Arrhythmia Database — the 
benchmark dataset used in real cardiology and biomedical engineering research.

The pipeline:
1. Downloads real ECG recordings and expert cardiologist annotations from PhysioNet
2. Applies bandpass filtering to remove baseline wander and powerline noise
3. Detects and segments individual heartbeats using expert-annotated R-peak locations
4. Extracts clinically relevant time-domain features (amplitude, shape statistics, RR interval)
5. Trains a Random Forest classifier to distinguish normal beats from arrhythmias 
   (premature ventricular contractions, atrial premature beats, fusion beats)
6. Evaluates performance using per-class sensitivity/recall rather than overall accuracy, 
   since missing a dangerous arrhythmia is far costlier than a false alarm

## Methodology Notes

- **Patient-level train/test split**, not beat-level, to prevent data leakage — since beats 
  from the same patient are highly correlated, a random beat-level split would inflate 
  performance metrics artificially.
- **Class-balanced training** to account for the natural imbalance toward normal beats in 
  the dataset.
- **Interpretable feature engineering** over raw waveform deep learning, prioritizing 
  explainability (feature importances are directly inspectable) given the clinical context.

## Limitations

MIT-BIH is a relatively small, decades-old dataset recorded from a limited patient 
population at a single institution. This is a research and educational pipeline, not a 
validated diagnostic tool. A real clinical deployment would require larger, more diverse, 
and more recent data, prospective clinical validation, and regulatory clearance (FDA 510(k) 
or De Novo pathway) before any clinical use.

## Data Source

[MIT-BIH Arrhythmia Database](https://physionet.org/content/mitdb/1.0.0/) via PhysioNet, 
accessed using the `wfdb` Python library.
