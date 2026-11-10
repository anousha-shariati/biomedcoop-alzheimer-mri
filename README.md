# BiomedCoOp for Alzheimer's disease MRI

Research records for my FOSE7901 Placement 2 project at Macquarie University (2026).

The project first reproduces BiomedCoOp (Koleilat et al., CVPR 2025), a prompt-learning method for biomedical vision-language models, on the brain tumour MRI dataset used in the original paper. It then applies the method to Alzheimer's disease on brain MRI with only a few labelled images per class, and compares it with zero-shot BiomedCLIP, CoOp and linear probes.

The results are discussed in the final report. Each notebook is saved with its outputs, so every run can be seen without running the code again.

## Notebooks

| Notebook | What it does |
|---|---|
| `01_BiomedCoOp_reproduction_BTMRI.ipynb` | Reproduces the published zero-shot and BiomedCoOp results on BTMRI (1 to 16 labelled images per class, seeds 1 to 3), and compares them with the paper using Welch's t-tests. |
| `02_BiomedCoOp_Alzheimer_MRI.ipynb` | Applies BiomedCoOp to four dementia stages on the `Falah/Alzheimer_MRI` dataset, with class descriptions written with an LLM, and checks the dataset. The dataset does not record which participant each slice belongs to, so it can only be split at the slice level. |
| `03_BiomedCoOp_OASIS1.ipynb` | The main experiments on OASIS-1, split by participant. Three classes (CDR 0 / 0.5 / ≥ 1) and two classes (no dementia vs dementia). It covers the choice of class names and descriptions on the validation set, stronger text guidance, a zero-shot-initialised linear probe, robustness checks (more participants, age), the choice of slices, and a cross-validated estimate on all 200 participants. |
| `04_BiomedCoOp_OASIS1_coronal.ipynb` | Repeats the main two-class experiments on coronal slices built from the original OASIS-1 volumes, for the same participants and split. |

Notebook 4 reads files that notebook 3 saves, so notebook 3 has to be run first.

## How to run

* Open a notebook in Google Colab with a T4 GPU and run all cells. The notebooks mount Google Drive and save everything to `MyDrive/placement2` (`outputs` for run logs, `results` for tables and figures).
* Finished runs are skipped, so after a disconnect all cells can be run again. Runs finished in an earlier session print "already done".
* Notebook 3 downloads data from Kaggle. If Kaggle asks for a login, run `kagglehub.login()` once and paste your API token.
* All experiments use the authors' code at commit [`23710b0`](https://github.com/HealthX-Lab/BiomedCoOp/tree/23710b0b867e8cb4a5ea95b0ccbaa5e35203db0d), with their settings. The new datasets and descriptions are added at run time, so the authors' files are not edited. The only change is one compatibility fix in Dassl: newer PyTorch versions no longer accept the `verbose` argument of its learning-rate scheduler, so it is removed (it is always `False`).
* The package versions used are printed in the last cells of the notebooks (Python 3.13, PyTorch 2.11 and 2.14, `open_clip_torch` 2.23.0, `transformers` 4.35.2).

## Data

The datasets are not included in this repository. They are publicly available from their original sources, and the notebooks download them:

* **BTMRI**: the brain tumour MRI dataset prepared by the BiomedCoOp authors ([Hugging Face: TahaKoleilat/BiomedCoOp](https://huggingface.co/datasets/TahaKoleilat/BiomedCoOp)), with their official split.
* **Falah/Alzheimer_MRI**: [Hugging Face](https://huggingface.co/datasets/Falah/Alzheimer_MRI), 6,400 axial slices in four classes.
* **OASIS-1** (Marcus et al., 2007): axial slices from [Kaggle: ninadaithal/imagesoasis](https://www.kaggle.com/datasets/ninadaithal/imagesoasis), the official table of participants with age and CDR from [Kaggle: jboysen/mri-and-alzheimers](https://www.kaggle.com/datasets/jboysen/mri-and-alzheimers), and the original volumes for the coronal slices from [oasis-brains.org](https://www.oasis-brains.org).

Data were provided by OASIS. OASIS-1: Cross-Sectional: Principal Investigators: D. Marcus, R. Buckner, J. Csernansky, J. Morris; P50 AG05681, P01 AG03991, P01 AG026276, R01 AG021910, P20 MH071616, U24 RR021382.

## Records

The full logs of every run, the K-shot samples, and the result tables and figures are kept in the project's Google Drive folder, with a copy on University OneDrive storage.

## Use of generative AI

The class descriptions were generated with Claude (Anthropic), using the same query as the BiomedCoOp authors (see notebook 2). Claude was also used to help write and debug the code. All experiments were run by the author.

## References

* Koleilat, T., Asgariandehkordi, H., Rivaz, H., and Xiao, Y. 2025. BiomedCoOp: Learning to Prompt for Biomedical Vision-Language Models. CVPR 2025, pp. 14766–14776. Code: https://github.com/HealthX-Lab/BiomedCoOp
* Zhang, S. et al. 2023. BiomedCLIP: a multimodal biomedical foundation model pretrained from fifteen million scientific image-text pairs. arXiv:2303.00915.
* Zhou, K., Yang, J., Loy, C. C., and Liu, Z. 2022. Learning to Prompt for Vision-Language Models (CoOp). International Journal of Computer Vision, 130:2337–2348.
* Marcus, D. S., Wang, T. H., Parker, J., Csernansky, J. G., Morris, J. C., and Buckner, R. L. 2007. Open Access Series of Imaging Studies (OASIS): Cross-sectional MRI data in young, middle aged, nondemented, and demented older adults. Journal of Cognitive Neuroscience, 19(9):1498–1507.
