# Consent-Aware Federated Learning for Health Data

Code accompanying the paper on consent withdrawal, fairness, privacy and erasure in federated healthcare AI.

## Repository structure

```text
.
├── notebooks/
│   ├── 01_uci_main_experiments.ipynb
│   ├── 02_uci_observation_validation.ipynb
│   ├── 03_temporal_mlp_fedprox.ipynb
│   ├── 04_nhanes_external_validation.ipynb
│   └── 05_generate_paper_figures.ipynb
├── figures/
├── results/
├── requirements.txt
├── .gitignore
```


## Paper experiment map

`01_uci_main_experiments.ipynb` contains the main UCI pipeline: preprocessing, fixed train/test split, nine simulated non-IID hospitals, centralised and FL baselines, feature-group masking, age-subgroup fairness, progressive consent restriction, and four-method feature-importance validation.

`02_uci_observation_validation.ipynb` contains the additional signal-concentration checks: Observation-only vs non-Observation-only, single-variable analysis, leave-one-variable-out ablation, top-2 Observation features, and the remaining eight Observation features.

`03_temporal_mlp_fedprox.ipynb` contains the neural extension used for temporal consent revocation, including FedProx, early/mid/late erasure, probability-change analysis, membership inference and drift attribution.

`04_nhanes_external_validation.ipynb` contains the external validation. Importantly, `HUQ051` is audited as a leakage-prone utilisation variable and the final cross-dataset interpretation uses the corrected analysis after removing it.

`05_generate_paper_figures.ipynb` reads the generated CSV result tables and creates the final paper figures.

## Data

Raw patient-level data are **not included**.

### UCI Diabetes

Place the UCI *Diabetes 130-US Hospitals for Years 1999–2008* file in the repository root as:

```text
diabetic_data.csv
```

### NHANES

Place the required NHANES source `.XPT` files where indicated in `04_nhanes_external_validation.ipynb`. The notebook documents the components and variables it loads.

## Environment

Python 3.10+ is recommended.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Recommended execution order

Run the notebooks in numeric order. The UCI main notebook creates intermediate tables consumed by the validation, temporal and figure-generation notebooks.

## Reproducibility notes

The experimental code uses fixed random seeds where defined in the original analysis. Generated result tables and figures should be written to repository-relative output directories rather than user-specific absolute paths.

The original research notebooks contained exploratory cells, repeated fixes, diagnostic reruns and some failed intermediate branches. Those are not part of this public release. The goal of this repository is to expose the final experiment logic used to support the paper, not the full development history.

## Important methodological notes


The NHANES validation should be interpreted using the corrected post-leakage analysis after removing `HUQ051`.

Membership-inference results near 0.5 indicate that the evaluated attack could not reliably distinguish members from non-members; they should not be described as a formal privacy guarantee.

