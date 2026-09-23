# Multi-Task Swin Transformers for Skin Lesion Classification

MSc Artificial Intelligence dissertation project, University of Bath.

This repository contains the notebooks used to compare a diagnosis-only
Swin-Tiny baseline with a full multi-task model using seven Derm7pt
dermoscopic attributes as auxiliary tasks. Seven leave-one-out multi-task
ablations examine the contribution of individual auxiliary tasks.

The project evaluates diagnostic performance, calibration and cross-dataset
behaviour. It is research code and is not intended for clinical use.

## Notebooks

- `01A_Derm7pt_EDA.ipynb` — Derm7pt preparation
- `01B_Notebook_PH2_EDA.ipynb` — PH2 preparation
- `Notebook_02_ISIC2019_EDA.ipynb` — ISIC 2019 preparation and split checks
- `Notebook03_BaselineModel_Seed42(1).ipynb` — diagnosis-only baseline
- `Notebook04_MultiTaskModel_Seed42(4).ipynb` — full multi-task model
- `Notebook04_MultiTaskModel_Ablations(7).ipynb` — leave-one-out ablations
- `Notebook05_Final_Evaluation(1).ipynb` — final evaluation
- `Notebook06_Final_Results_and_Confusion_Analysis.ipynb` — results review and confusion analysis

## Experiments

- **Baseline:** diagnosis-only Swin-Tiny
- **Full MTL:** shared Swin-Tiny encoder, diagnostic head and seven Derm7pt attribute heads
- **Ablations:** seven leave-one-out MTL runs
- **Main comparison:** seeds 41, 42 and 43
- **Ablations:** seed 42
- **External evaluation:** mapped Derm7pt test data and inference-only PH2

## Data

Datasets are not included in this repository. Obtain them from the original
providers and follow their current licences and conditions of use.

- [ISIC 2019](https://challenge.isic-archive.com/data/)
- [Derm7pt](http://derm.cs.sfu.ca/)
- [Derm7pt repository](https://github.com/jeremykawahara/derm7pt)
- [PH2](https://www.fc.up.pt/addi/ph2%20database.html)

Please cite the original dataset publications when using these data. The
dissertation bibliography contains the references used in this project.

## Paths and outputs

The notebooks were developed in Google Colab with Google Drive paths rooted at
`Dissertation_Data`.

```text
Dissertation_Data/
├── Baseline/
│   ├── checkpoints/
│   ├── figures/
│   ├── predictions/
│   └── results/
├── MultiTask/
│   ├── checkpoints/
│   ├── figures/
│   ├── predictions/
│   ├── results/
│   └── auxiliary_results/
└── FinalEvaluation_Folder/
    ├── figures/
    ├── predictions/
    ├── tables/
    └── Detailed_Outputs/
        ├── figures/
        └── tables/
```

`MultiTask` contains outputs from the full MTL runs and the leave-one-out
ablations. Notebook 05 produces the final evaluation outputs, while Notebook 06
uses saved predictions to produce the additional confusion-analysis outputs.

Large datasets, checkpoints, predictions and generated outputs are not included
in this repository.

## Scope

Results are specific to the models, splits and datasets evaluated in the MSc
dissertation. This repository is research code, not clinical software.
