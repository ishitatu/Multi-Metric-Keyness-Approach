[README.md](https://github.com/user-attachments/files/34671669/README.md)
# Multi-Metric Keyness Approach

Code and results for identifying **move-specific key 4-grams** in move-annotated corpora.

This repository accompanies the article

> Ishii, T., et al. (under review). *Identifying move-specific key 4-grams in experimental medical research article Introductions: A multi-metric keyness approach.*

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ishitatu/Multi-Metric-Keyness-Approach/blob/main/6_Supplementary_Material_S2_analysis_script.ipynb)

## Contents

| File | Description |
|---|---|
| `6_Supplementary_Material_S2_analysis_script.ipynb` | **Supplementary Material S2.** Analysis script (Jupyter / Google Colab). Extracts the key 4-grams of each move and produces Tables 1, 3, 4, 5, and A1–A4 of the article. |
| `5_Supplementary_Material_S1_results.xlsx` | **Supplementary Material S1.** Complete results: all 177 key 4-gram entries with their statistics, LBiaP counts, structural types, step coding, and the concordance lines on which the coding is based. |

## Method in brief

A 4-gram is a **key 4-gram** of a move (the target) when it meets four criteria at the same time, each measured against all other moves of the corpus (the reference):

| Criterion | Definition | Threshold |
|---|---|---|
| Recurrence | raw frequency in the target | ≥ 3 occurrences in ≥ 3 texts |
| Frequency keyness (FK) | log-likelihood G² on token counts (Dunning, 1993) | G² ≥ 3.84 (*p* < .05) |
| Text dispersion keyness (TDK) | log-likelihood G² on document counts (Egbert & Biber, 2019) | G² ≥ 3.84 (*p* < .05) |
| Mean text frequency keyness (MTK) | Cohen's *d* on per-text frequencies per 1,000 tokens (Egbert & Biber, 2023; Larsson et al., 2025) | *d* > 0 |

Design choices (all are defaults in the script and reproduce the article):

- **Reference corpus**: the whole corpus minus the target move, so that target and reference share no observations.
- **G²** is computed on the full 2 × 2 contingency table (a two-term option, as in Larsson et al.'s keylexbundles, is available).
- **Cohen's *d*** uses the unweighted pooled standard deviation, √((SD_T² + SD_R²)/2).
- **Statistics are computed on raw surface frequencies.** Overlapping windows are not filtered out; instead, each 4-gram is annotated with its count under the interlocking control of **LBiaP** (Cortes & Lake, 2023). The annotation alters no statistic.
- **Tokenization**: texts are lowercased; hyphens and apostrophes are word-internal (typographic variants normalized); non-ASCII letters count as letters (*NF-κB* = one token).
- **Windows** do not cross `. ? ! ; :`; all other punctuation is transparent (*here, we show that* = *here we show that*).
- All decisions are made on unrounded values.

## How to run

### 1. Prepare the corpus

Put the whole corpus in **one ZIP file**. Folders represent sections and, below them, moves; each text is a plain `.txt` file (UTF-8). For example:

```
corpus.zip
├── 1_Introduction/
│   ├── MOVE1/   Cell 2014-1 MOVE1.txt, ...
│   ├── MOVE2/   ...
│   └── MOVE3/   ...
├── 2_Methods/
│   └── ...
├── 3_Results/
│   └── ...
└── 4_Discussion/
    └── ...
```

Hidden files and folders (`.DS_Store`, `._*`, `__MACOSX`) are ignored. Citation markers and reference lists should be removed beforehand.

### 2. Extract the key 4-grams (first code cell)

1. Click **Open in Colab** above (or open the notebook in Jupyter).
2. Run the first code cell and answer the prompts. **Press Enter to accept every default; the defaults reproduce the article.** The prompts set the n-gram length, the tokenizer options, the G² variant, the pooled SD, the reference design, the recurrence thresholds, and the LBiaP annotation.
3. Upload the corpus ZIP. The folders are listed with their numbers of `.txt` files, and target folders are suggested (the sub-folders of the first section, e.g. the three Introduction moves). Press Enter to accept, or type other folder numbers (e.g. `6,7,8` or `6-8`) or `all`.

Each target is analysed separately against the rest of the corpus. The script downloads:

- `key4grams_<folder>.xlsx` for each target, with the sheets
  - `settings`
  - `extracted 4-grams`: all candidates meeting the recurrence criterion, with FK, TDK, MTK, LBiaP counts, and overlap families
  - `key 4-grams (Table A)`: the move-specific key 4-grams in the format of Appendix Tables A2–A4, with empty `Type` and `Step` columns for coding
  - `criteria summary`: counts retained by each criterion alone and by the four jointly
- `summary_tables.xlsx` with Table 1 (corpus description), Table 3 (criteria), and Table A1 (key 4-grams at *p* < .05, .01, .001, and .0001).

Folder names are displayed as IM1–IM3 through the `LABEL` dictionary in the code; edit it for other data.

### 3. Code the key 4-grams and build the result tables (second code cell)

Fill in the `Type` and `Step` columns of each `key 4-grams (Table A)` sheet:

- `Type`: subtype code of Biber, Conrad, and Cortes (2004, p. 381, Table 2): `1a`–`1g`, `2a`–`2e`, `3a`–`3e`
- `Step`: `Step 1`, `Step 2`, … (numbers restart within each move), or `NS` (not step-signalling)

Then run the second code cell and upload the coded workbook(s). It writes `result_tables.xlsx` with Table 4 (structural types by move), Table 5 (steps × structural types, with representative 4-grams), and one Table A sheet per move. Move and step names can be edited in `MOVE_CODE`, `MOVE_TITLE`, and `STEP_TITLE` at the top of the cell.

### Running locally

The notebook also runs outside Colab with Python 3, `pandas`, and `openpyxl`:

```bash
pip install pandas openpyxl
jupyter notebook 6_Supplementary_Material_S2_analysis_script.ipynb
```

Outside Colab, the script asks for the path to the corpus ZIP (or folder) instead of an upload, and writes the Excel files to the working directory.

## Supplementary Material S1: sheets

| Sheet | Content |
|---|---|
| `Settings` | Tokenizer, boundaries, reference, and keyness settings |
| `T1 Corpus` | Table 1: target and reference corpora |
| `T2 Steps (Nwogu)` | Table 2: Introduction moves and steps (Nwogu, 1997) |
| `T3 Criteria` | Table 3: candidates retained by each criterion and jointly |
| `T4 Biber types` | Table 4: structural types of the key 4-grams by move |
| `T5 Steps x types` | Table 5: steps signalled by the key 4-grams and their structural types |
| `A1 Sensitivity` | Table A1: inventories at four significance levels |
| `A2 Key 4-grams IM1` – `A4 Key 4-grams IM3` | Tables A2–A4: key 4-grams of each move |
| `Exclusivity (3.1)` | Number of moves in which the key 4-grams occur |
| `Dual-key types` | 4-grams key in more than one move |
| `All 177 entries` | All key 4-gram entries with statistics, LBiaP counts, and frequencies in each move |
| `Step coding` | Structural type and step assigned to each entry |
| `Concordance lines` | Sentences from the corpus on which the step coding is based |

## Data

The corpus consists of the Introduction, Methods, Results, and Discussion sections of 300 experimental medical research articles published in 2014 in 30 peer-reviewed life-science journals, annotated for moves following Nwogu (1997). The source articles are protected by copyright, so the corpus itself is not redistributed. S1 contains only the sentences needed to verify the step coding.

## How to cite

Please cite the article above. A full reference will be added here on publication.

## License

The code (S2) is released under the MIT License, and the results (S1) under CC BY 4.0.

## References

- Biber, D., Conrad, S., & Cortes, V. (2004). If you look at …: Lexical bundles in university teaching and textbooks. *Applied Linguistics, 25*(3), 371–405. https://doi.org/10.1093/applin/25.3.371
- Cortes, V., & Lake, W. (2023). LBiaP: A solution to the problem of attaining observation independence in lexical bundle studies. *International Journal of Corpus Linguistics, 28*(2), 263–277. https://doi.org/10.1075/ijcl.21100.cor
- Dunning, T. (1993). Accurate methods for the statistics of surprise and coincidence. *Computational Linguistics, 19*(1), 61–74.
- Egbert, J., & Biber, D. (2019). Incorporating text dispersion into keyword analyses. *Corpora, 14*(1), 77–104. https://doi.org/10.3366/cor.2019.0162
- Egbert, J., & Biber, D. (2023). Key feature analysis: A simple, yet powerful method for comparing text varieties. *Corpora, 18*(1), 121–133. https://doi.org/10.3366/cor.2023.0275
- Larsson, T., Kim, T., & Egbert, J. (2025). Introducing and comparing two techniques for key lexical bundles analysis. *Research Methods in Applied Linguistics, 4*(3), Article 100245. https://doi.org/10.1016/j.rmal.2025.100245
- Nwogu, K. N. (1997). The medical research paper: Structure and functions. *English for Specific Purposes, 16*(2), 119–138. https://doi.org/10.1016/S0889-4906(97)85388-4

## Contact

Tatsuya Ishii, Faculty of Humanities and Social Sciences, Kochi University, Japan
