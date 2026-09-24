# Multi-Task Swin Transformers for Clinical Feature Alignment in Skin Lesion Diagnosis

MSc Artificial Intelligence dissertation project, University of Bath.

This repository contains the training and evaluation notebooks used to compare a diagnosis-only Swin-Tiny baseline with a multi-task Swin-Tiny model that jointly predicts skin-lesion diagnosis and seven Derm7pt clinical attributes.

## Experimental design

The main comparison uses the same Swin-Tiny encoder architecture in both configurations:

- **Baseline:** trained on ISIC 2019 for three-class diagnosis only.
- **Full MTL:** trained with the ISIC 2019 diagnostic objective and seven Derm7pt auxiliary attribute heads.
- **Leave-one-out ablations:** seven seed-42 multi-task runs, each omitting one auxiliary task.

The main baseline and full MTL experiments use seeds **41, 42 and 43**. The leave-one-out ablations use **seed 42**.

The final experiment registry therefore contains **13 runs**:

- 3 baseline runs
- 3 full multi-task runs
- 7 leave-one-out ablations

## Datasets

The project uses:

- **ISIC 2019** for primary diagnosis training, validation and testing.
- **Derm7pt** for auxiliary clinical-attribute supervision and mapped diagnostic evaluation.
- **PH2** as an inference-only external diagnostic evaluation.

Dataset files are not included in this repository. The notebooks expect the project data to be available under a configurable project root. In the original Google Colab experiments this was:

```text
/content/drive/MyDrive/Dissertation_Data
```

> **Important:** `Derm7pt/release_v0/` is the name of the Derm7pt dataset release used by the notebooks. It is not an obsolete experiment version and should not be confused with the project's `v2` run names.

## Notebooks

For a clean GitHub repository, the final notebooks can be named:

```text
Notebook03_BaselineModel.ipynb
Notebook04_MultiTaskModel.ipynb
Notebook04_MultiTaskModel_Ablations.ipynb
Notebook05_Final_Evaluation.ipynb
Notebook06_Final_Results_and_Confusion_Analysis.ipynb
```

### Notebook 03 — Baseline model

Trains the diagnosis-only Swin-Tiny model on ISIC 2019 and saves checkpoints, training histories, figures and predictions.

Final run naming:

```text
baseline_final_v2_seed_41
baseline_final_v2_seed_42
baseline_final_v2_seed_43
```

### Notebook 04 — Full multi-task model

Trains the full multi-task model using the ISIC diagnosis objective together with seven Derm7pt auxiliary tasks.

Final run naming:

```text
mtl_final_v2_seed_41
mtl_final_v2_seed_42
mtl_final_v2_seed_43
```

### Notebook 04 — Leave-one-out ablations

Runs the seven exploratory seed-42 ablations, each removing one Derm7pt auxiliary task.

Final run naming follows:

```text
mtl_ablation_v2_no_<auxiliary_task>_seed_42
```

### Notebook 05 — Final evaluation

Loads the saved final `v2` model outputs and produces the dissertation evaluation tables, figures and external-dataset predictions.

It evaluates:

- three-class ISIC performance
- malignant-versus-rest AUROC and operating points
- melanoma-versus-rest AUROC and operating points
- calibration
- auxiliary-task performance
- leave-one-out ablations
- mapped Derm7pt diagnostic performance
- PH2 diagnostic performance
- transported 90% and 95% sensitivity operating points

### Notebook 06 — Final results and confusion analysis

Reads the saved Notebook 05 outputs and produces the detailed confusion analyses and supplementary tables used for the final Results and Discussion chapters.

## Output structure

The notebooks use the following structure under `Dissertation_Data/`:

```text
Dissertation_Data/
├── Baseline/
│   ├── checkpoints/
│   ├── figures/
│   ├── results/
│   └── predictions/
│
├── MultiTask/
│   ├── checkpoints/
│   ├── figures/
│   ├── results/
│   ├── predictions/
│   └── auxiliary_results/
│
└── FinalEvaluation_Folder/
    ├── tables/
    ├── figures/
    ├── predictions/
    └── Detailed_Outputs/
        ├── tables/
        └── figures/
```

### Model artefacts

Baseline checkpoints:

```text
Baseline/checkpoints/
```

Full MTL and ablation checkpoints:

```text
MultiTask/checkpoints/
```

Checkpoints follow the pattern:

```text
<RUN_NAME>_best.pt
```

### Training results

Baseline:

```text
Baseline/results/
Baseline/figures/
Baseline/predictions/
```

Multi-task and ablations:

```text
MultiTask/results/
MultiTask/figures/
MultiTask/predictions/
MultiTask/auxiliary_results/
```

### Final evaluation outputs

Notebook 05 writes the final dissertation evaluation artefacts to:

```text
FinalEvaluation_Folder/tables/
FinalEvaluation_Folder/figures/
FinalEvaluation_Folder/predictions/
```

Notebook 06 writes the additional confusion-analysis outputs to:

```text
FinalEvaluation_Folder/Detailed_Outputs/tables/
FinalEvaluation_Folder/Detailed_Outputs/figures/
```

The final tables should be treated as generated analysis outputs rather than independent hand-edited result files.

## Reproducibility

The final experiments use:

- Swin-Tiny (`swin_tiny_patch4_window7_224`)
- 224 × 224 input images
- seeds 41, 42 and 43 for the main comparison
- seed 42 for leave-one-out ablations
- AdamW optimisation
- validation-based checkpoint selection and early stopping

The notebooks set deterministic random seeds for the main training components. Results are reported across the three final main seeds where applicable.

## Recommended execution order

```text
1. Prepare the ISIC 2019 and Derm7pt data/splits
2. Run Notebook03 for baseline seeds 41, 42 and 43
3. Run Notebook04 full MTL for seeds 41, 42 and 43
4. Run Notebook04 ablations for the seven seed-42 leave-one-out configurations
5. Run Notebook05_Final_Evaluation
6. Run Notebook06_Final_Results_and_Confusion_Analysis
```

Notebook 05 is intentionally restricted to the final `v2` experiment registry so that older exploratory runs are not included in the dissertation results.

## Repository hygiene

For the public/final repository:

- keep only the final notebooks and final `v2` analysis outputs;
- remove or archive obsolete experiment artefacts whose run names contain `v1`;
- do **not** remove `Derm7pt/release_v0/`, because that is the dataset release name;
- avoid committing duplicate notebook copies such as `(1)`, `(2)`, `(3)(4)` or similar local-download suffixes;
- do not commit the original image datasets;
- consider excluding large `.pt` checkpoints from normal Git history or storing them with Git LFS if they must be distributed.

A suitable `.gitignore` should normally exclude raw datasets, temporary Colab files, Python caches and any checkpoints that are not intended for distribution.

## Scope

The study is an empirical comparison of complete training configurations. Because the full multi-task model also receives Derm7pt training images and uses a joint multi-task objective, differences between the baseline and full MTL configuration should not be interpreted as isolating the causal effect of the auxiliary labels alone.
