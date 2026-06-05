# ftm_mpH2MM — Single-Molecule FRET / mpH2MM Analysis

Jupyter notebooks for single-molecule FRET (smFRET) data analysis using
**mpH2MM** (multiparameter Hidden Markov Modelling) with Alexa 555 / Alexa 647
dye pairs measured by ns-ALEX (nanosecond Alternating Laser EXcitation).

---

## What this repo does

Raw PicoQuant `.ptu` files are converted to Photon-HDF5, burst-searched, and
analysed with H²MM to extract conformational states and transition dynamics:

1. Convert multiple `.ptu` files → merged Photon-HDF5 (`phconvert`)
2. Filter junk detector channels (keep det 0 & det 1 only)
3. Burst search + selection (`FRETbursts`)
4. First-round H²MM → remove donor-only and acceptor-only bursts
5. Second-round H²MM → identify genuine FRET states
6. Dwell-time extraction and survival plots
7. Burst Variance Analysis (BVA) to confirm conformational dynamics

---

## Dye / laser configuration

| | Donor | Acceptor |
|---|---|---|
| Dye | Alexa 555 | Alexa 647 |
| Excitation | 520 nm | 640 nm |
| Emission | 570 nm | 690 nm |
| Detector | det 1 | det 0 |
| ALEX window | 0 – 1000 ticks | 1000 – 2000 ticks |

---

## Repo structure

```
ftm_mpH2MM/
├── notebooks/
│   ├── H2MM_PTU_Alexa555_647_MULTIFILE.ipynb   ← main analysis (multi-PTU)
│   ├── H2MM_PTU_Alexa555_647_FIXED.ipynb        ← single-file version
│   ├── H2MM_PTU_Alexa555_647_CORRECTED.ipynb    ← earlier corrected version
│   ├── H2MM_CSV_Alexa555_647.ipynb              ← CSV-based analysis
│   └── H2MM_CSV_Alexa555_647_final.ipynb        ← final CSV version
├── data/                                         ← NOT tracked by git
│   ├── raw/                                      ← PTU files (in Dropbox)
│   └── processed/                                ← HDF5 files (in Dropbox)
├── results/                                      ← figures and output plots
├── .gitignore
├── LICENSE
└── README.md
```

> **Note:** Raw PTU files and processed HDF5 files are stored in Dropbox,
> not in this repository. Only notebooks and results are version-controlled.

---

## Key burst search parameters

Aligned with reference scripts (`calculate_BVA.py`, `search_all.py`):

| Parameter | Value | Source |
|---|---|---|
| Background fit | `exp_hist_fit`, 60 s, F_bg=1.7 | `calculate_BVA.py` |
| Burst search m | 15 | `calculate_BVA.py` |
| Ph_sel | `Ph_sel(Dex='DAem')` | `calculate_BVA.py` |
| min naa | 30 | `calculate_BVA.py` |
| min burst size | 150 | `calculate_BVA.py` |
| Stoichiometry filter | S ∈ [0.25, 0.85] | `calculate_BVA.py` |
| BVA chunk size | 7 | `calculate_BVA.py` |

---

## Dependencies

Install into your conda/uv environment:

```bash
pip install fretbursts phconvert burstH2MM H2MM_C numpy matplotlib h5py
```

Or with conda:

```bash
conda install -c conda-forge fretbursts phconvert
pip install burstH2MM H2MM_C
```

Tested with:
- Python 3.10
- FRETbursts 0.7+
- phconvert 0.9+
- burstH2MM 0.2+
- H2MM_C 0.3+

---

## How to run

1. Clone the repo:
```bash
git clone https://github.com/fatmasen/ftm_mpH2MM.git
cd ftm_mpH2MM
```

2. Open the main notebook:
```
notebooks/H2MM_PTU_Alexa555_647_MULTIFILE.ipynb
```

3. In Cell 6, update the paths to point to your PTU files and output folder:
```python
data_dir  = Path(r'path/to/your/ptu/files')
data_dir2 = Path(r'path/to/save/hdf5/files')
```

4. Run all cells top to bottom (`Ctrl+F9` in VS Code)

---

## Author

Fatma — KU Leuven
