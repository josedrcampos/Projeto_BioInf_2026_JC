# A DBTL-based Bioinformatics Pipeline for Comparative Metabolic Pathway Annotation in Microalgae

A reproducible, open-source pipeline that applies the **Design–Build–Test–Learn (DBTL)** paradigm to the comparative annotation of metabolic pathways in two microalgae, *Dunaliella salina* (FACHB435) and *Nannochloropsis gaditana* (B-31), with a focus on **carotenoid** and **omega-3 fatty-acid** biosynthesis.

**Author:** José Campos · **Supervisor:** Óscar Dias
**Centre of Biological Engineering, School of Engineering, University of Minho, Braga, Portugal**

---

## Overview

Comparative metabolic pathway annotation across microalgal species is hampered by data heterogeneity, incomplete genome annotations, and the absence of reproducible analytical frameworks. This project addresses those gaps with a modular Snakemake pipeline that integrates two complementary annotation strategies and benchmarks them against a curated gold standard. From the raw protein FASTA files, a single command regenerates every figure and table in the accompanying article.

The four DBTL phases map onto the pipeline as follows:

| Phase | What it does |
|-------|--------------|
| **Design** | Data acquisition, tool selection, and definition of the target pathways |
| **Build** | Snakemake workflow integrating KofamScan (KEGG Orthology) and DIAMOND (Swiss-Prot homology), plus reCOGnizer (COG) and KEGGCharter (visualisation) |
| **Test** | Benchmarking of KofamScan against a Swiss-Prot gold standard (sensitivity, specificity, precision, F1) |
| **Learn** | Cross-species KO comparison, pathway-coverage analysis, and desaturase validation against a published genome-scale metabolic model |

---

## Key Results

| Metric | *D. salina* FACHB435 | *N. gaditana* B-31 |
|--------|:---:|:---:|
| Proteins analysed | 16,697 | 10,929 |
| KofamScan KOs assigned | 3,366 (20.2%) | 3,199 (29.3%) |
| DIAMOND (Swiss-Prot) annotated | 42.2% | 51.8% |
| Benchmark F1 score | 0.61 | 0.65 |
| Benchmark specificity | 0.93 | 0.88 |

- **397** KEGG pathways evaluated for coverage (complete / partial / absent).
- **1,709** KOs shared between species; **1,730** species-exclusive (907 + 823).
- **Desaturase validation:** *D. salina* encodes a complete PUFA desaturase set; *N. gaditana* B-31 lacks the Δ4- and ω3-desaturases reported for strain CCMP1894 — a strain-level annotation gap rather than a true biological absence.

---

## Repository Structure

```
.
├── README.md
├── LICENSE
├── environment.yml                 # conda environment specification
│
├── article/
│   ├── article_initial_phase1.pdf  # Phase 1 (proposal/intercalar) article
│   ├── article_final_phase3.pdf    # Phase 3 (final) article
│   └── latex/                      # LaTeX source (LLNCS) + figures
│       ├── article_DBTL_microalgae_FINAL.tex
│       └── figures/
│
├── presentation/
│   └── apresentacao_pipeline.pptx  # Phase 2 presentation slides
│
├── notebook/
│   └── microalgae_pipeline_notebook.ipynb   # full, runnable pipeline
│
├── workflow/
│   └── Snakefile                   # Snakemake workflow definition
│
├── data/
│   └── proteins/                   # input protein FASTA files
│
├── results/
│   ├── kofamscan/                  # KO assignments per species
│   ├── diamond/                    # Swiss-Prot homology hits
│   ├── recognizer/                 # COG annotations
│   ├── comparison/                 # cross-species KO comparison + figures
│   ├── keggcharter/                # KEGG pathway maps
│   └── benchmark/                  # benchmarking metrics
│
└── supplementary/
    └── tool_comparison_matrices/   # per-phase bio.tools comparison matrices
```

---

## Data Sources

| Species | Strain | Source | Identifier |
|---------|--------|--------|-----------|
| *Dunaliella salina* | FACHB435 | UniProtKB | taxon 3046 |
| *Nannochloropsis gaditana* | B-31 | NCBI | BioProject PRJNA170989 |

Target pathways: **carotenoid biosynthesis** (`ko00906`) and **biosynthesis of unsaturated fatty acids** (`ko01040`).

---

## Tools

All tools are open-source.

| Tool | Role | DBTL phase |
|------|------|-----------|
| [Snakemake](https://snakemake.readthedocs.io/) | Workflow management | Build |
| [KofamScan](https://github.com/takaram/kofam_scan) | KEGG Orthology (KO) assignment via HMM profiles | Build |
| [DIAMOND](https://github.com/bbuchfink/diamond) | Homology annotation against Swiss-Prot | Build / Test |
| [reCOGnizer](https://github.com/iquasere/reCOGnizer) | COG functional annotation | Build |
| [KEGGCharter](https://github.com/iquasere/KEGGCharter) | KEGG pathway-map visualisation | Learn |

Written in **Python 3.12** with Biopython, pandas, Matplotlib, seaborn, requests and matplotlib-venn.

---

## Reproducing the Analysis

### 1. Clone the repository

```bash
git clone https://github.com/josedrcampos/<REPO-NAME>.git
cd <REPO-NAME>
```

### 2. Create the environment

```bash
conda env create -f environment.yml
conda activate microalgae-dbtl
```

### 3. Run the pipeline

**Option A — Snakemake workflow:**

```bash
cd workflow
snakemake --cores 4
```

**Option B — Jupyter notebook (step by step):**

```bash
jupyter notebook notebook/microalgae_pipeline_notebook.ipynb
```

Then run *Kernel → Restart & Run All*. All outputs are written to `results/`.

> **Note:** KEGG pathway mapping queries the KEGG REST API. Responses are cached locally on first run, so re-execution recomputes only the affected steps.

---

## The Article

The final article is written in the **Springer Lecture Notes in Computer Science (LLNCS)** format. The LaTeX source compiles in Overleaf without modification (the `llncs.cls` class is bundled there). To compile locally, ensure the LLNCS class is installed and place the figures in `article/latex/figures/`.

---

## Citation

If you use this pipeline, please cite:

> Campos, J., Dias, Ó.: A DBTL-based Bioinformatics Pipeline for Comparative Metabolic Pathway Annotation in Microalgae. University of Minho (2025).

### Key references

- Carbonell, P. et al. (2018). An automated Design–Build–Test–Learn pipeline for enhanced microbial production of fine chemicals. *Communications Biology* 1, 66.
- Sequeira, J.C. et al. (2022). UPIMAPI, reCOGnizer and KEGGCharter: bioinformatics tools for functional annotation and visualization of (meta)-omics datasets. *Computational and Structural Biotechnology Journal* 20, 1798–1810.
- Cunha, E.R. (2025). Reconstruction of genome-scale metabolic models of microalgae for exploration of pigments and lipids production. PhD thesis, University of Minho. <https://hdl.handle.net/1822/97481>

---

## License

This project is released under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## Acknowledgements

Developed at the Centre of Biological Engineering, University of Minho, under the supervision of Óscar Dias. The functional-annotation stack (UPIMAPI, reCOGnizer, KEGGCharter, MOSCA) and the `merlin` modelling platform developed at the University of Minho informed the design of this pipeline.
