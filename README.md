# wilson-eeg-rsa-replication

Replication and extension of Wilson et al.'s EEG-based Representational Similarity Analysis (RSA) pipeline using the publicly available [ERP CORE dataset](https://erpinfo.org/erp-core).

---

## Repository Structure

```
.
├── notebooks/
│   └── rsa_pipeline.ipynb   # Core RSA pipeline (see below)
└── README.md
```

---

## RSA Pipeline (`notebooks/rsa_pipeline.ipynb`)

This notebook implements a complete, reproducible RSA pipeline for EEG data. It is designed to demonstrate the core steps involved in multivariate EEG analysis aligned with the goals of the Wilson et al. replication.

### Pipeline Overview

| Step | Description |
|------|-------------|
| 1. Data Loading | Loads ERP CORE N170 data (faces vs. cars) via MNE-Python's built-in dataset utilities. No manual download required. |
| 2. Preprocessing | Band-pass filtering (0.1–40 Hz), average re-referencing, epoch extraction (−200 to 800 ms), baseline correction (−200 to 0 ms). |
| 3. Neural RDMs | Constructs time-resolved Representational Dissimilarity Matrices (RDMs) using pairwise correlation distance (1 − Pearson r) across condition-averaged EEG spatial patterns. Output shape: `(n_times × n_conditions × n_conditions)`. |
| 4. Model RDMs | Constructs two model RDMs: a **Category model** (face vs. car) and an **Identity null model** (uniform dissimilarity). |
| 5. Time-Resolved RSA | Computes Spearman rank correlation between the neural RDM upper triangle and each model RDM at every time point, producing an RSA time course. |
| 6. Visualization | Plots model RDMs, neural RDM at the N170 peak (~170 ms), and RSA time courses with p < 0.05 significance shading. |

### Dependencies

- [MNE-Python](https://mne.tools) ≥ 1.6
- NumPy, SciPy, Matplotlib, Seaborn, tqdm

Install with:

```bash
pip install mne numpy scipy matplotlib seaborn tqdm
```

### Dataset

The notebook uses the **ERP CORE N170** component dataset (Kappenman et al., 2021), which is freely available and automatically fetched by MNE:

> Kappenman, E. S., Farrens, J. L., Zhang, W., Stewart, A. X., & Luck, S. J. (2021).  
> ERP CORE: An open ERP dataset and pipeline for key components.  
> *NeuroImage*, 225, 117465. https://doi.org/10.1016/j.neuroimage.2020.117465

### Expected Results

When the Category model RSA coefficient peaks around 150–200 ms post-stimulus, this reflects the well-known N170 sensitivity to face vs. object category — consistent with the Wilson et al. replication target.

---

## Planned Extensions

- Permutation testing for robust null distributions
- Model comparison: pixel-level and DCNN-layer RDMs
- Group-level RSA across all 40 ERP CORE participants
- Temporal Generalization (cross-time RSA matrix)
- Searchlight RSA across electrode neighborhoods

---

*This repository is maintained as part of a thesis project on EEG and visual representational geometry at Kennesaw State University.*
