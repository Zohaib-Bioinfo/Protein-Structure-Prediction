# Protein Structure Prediction — Human Insulin (P01308)

Structure prediction of human insulin using AlphaFold2, with interpretation of model confidence metrics and comparison to the protein's known biological architecture.

## Overview

Human insulin (UniProt: [P01308](https://www.uniprot.org/uniprotkb/P01308)) is a small, well-characterized hormone consisting of two peptide chains — an A chain (21 residues) and a B chain (30 residues) — held together by two interchain disulfide bonds, with a third intrachain disulfide bond stabilizing the A chain. Its compact size and well-documented experimental structures make it a useful benchmark for evaluating structure prediction accuracy and confidence metrics.

This project runs AlphaFold2 on the insulin sequence and evaluates the resulting model using pLDDT (per-residue confidence), PAE (inter-residue positional error), and MSA coverage — the standard quality metrics for any AlphaFold2 prediction.

## Methodology

| Step | Detail |
|---|---|
| Tool | AlphaFold2, via the [ColabFold](https://github.com/sokrypton/ColabFold) `AlphaFold2.ipynb` notebook (Mirdita et al., 2022) |
| Input | UniProt sequence P01308 (set via `job_name`) |
| MSA construction | MMseqs2 search against UniRef + BFD/MGnify databases |
| Hardware | Google Colab, T4 GPU |
| Runtime | ~3 minutes |
| Output | Ranked models + confidence plots, packaged as a downloadable zip |

An initial attempt to run AlphaFold2 in a manually configured Colab environment encountered dependency and runtime errors. Switching to the maintained ColabFold notebook resolved this and completed the prediction successfully.

## Results

| File | Description |
|---|---|
| `results/P01308_Insulin_BestModel.pdb` | Predicted 3D structure (rank 1, unrelaxed) |
| `figures/plddt_plot.png` | Per-residue confidence (pLDDT > 90 across most of the structure) |
| `figures/pae_plot.png` | Predicted Aligned Error — inter-domain positional confidence |
| `figures/coverage_plot.png` | MSA depth across the sequence |
| `figures/insulin_3D.png` | Rendered 3D structure |
| `data/uniref.a3m`, `data/bfd.mgnify30.metaeuk30.smag30.a3m` | Raw MSA evidence used for prediction |

**Interpretation:** A pLDDT score above 90 indicates very high per-residue confidence, consistent with insulin's small size and well-represented sequence in existing structural databases. The predicted two-chain, disulfide-stabilized fold matches the known architecture of mature insulin.

## Repository Structure

```
Protein-Structure-Prediction/
├── README.md
├── LICENSE
├── .gitignore
├── results/
│   └── P01308_Insulin_BestModel.pdb
├── figures/
│   ├── plddt_plot.png
│   ├── pae_plot.png
│   ├── coverage_plot.png
│   └── insulin_3D.png
└── data/
    ├── uniref.a3m
    └── bfd.mgnify30.metaeuk30.smag30.a3m
```

## Reproducing This

This prediction was generated using the [ColabFold](https://github.com/sokrypton/ColabFold) `AlphaFold2.ipynb` notebook by Mirdita, Ovchinnikov, and Steinegger. To reproduce or run a different protein:

1. Open the notebook: [colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb](https://colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb)
2. Set `query_sequence` to your protein of interest and `job_name` to a label of your choice
3. Run all cells (GPU runtime required)
4. Download the resulting zip containing the `.pdb` model, confidence plots, and MSA files

**Citation:** Mirdita M, Schütze K, Moriwaki Y, Heo L, Ovchinnikov S, Steinegger M. *ColabFold: Making protein folding accessible to all.* Nature Methods (2022).

## Tools & Technologies

AlphaFold2, ColabFold, MMseqs2, Google Colab (T4 GPU), UniProt

## Limitations & Future Work

- Single-chain prediction without explicit modeling of insulin's known dimer/hexamer assembly states
- No structural alignment performed against experimental insulin structures (e.g., PDB [4INS](https://www.rcsb.org/structure/4INS)) to quantify RMSD
- Future extension: benchmark AlphaFold2 confidence against experimentally resolved structures for a set of proteins of varying size and disorder

## License

MIT License — see `LICENSE`

## Author

Muhammad Zohaib — BS Bioinformatics, University of Agriculture Faisalabad (UAF)
