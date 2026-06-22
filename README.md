# NSF CSE Funding Analysis (2021–2025)

An exploratory analysis of National Science Foundation (NSF) **Computer & Information Science and Engineering (CISE)** grant funding from 2021 to 2025. The project follows where NSF CSE dollars went across years, divisions, institutions, states, and principal investigators, and uses topic modeling on grant abstracts to map the research landscape.

## Key Findings

- **Funding peaked in 2023** (~$670M) and declined to ~$440M by 2025 — a ~34% drop from peak.
- **~$2.8B** in cumulative CSE funding over the five-year window.
- **Highly concentrated**: the top 100 institutions account for ~76.7% of all funding; dollars cluster in CA, NY, TX, IL, and the Northeast.
- **AI Agents & Healthcare** and **Academic Community & Outreach** are the fastest-growing subfields; **Privacy/Fairness** and **Networking & Edge** contract the most.
- **2023 anomaly**: STEM Education & Workforce spiked to ~$124M, likely tied to a workforce-funding push.

## Repository Contents

| File | Description |
|------|-------------|
| `NSF_Funding_Analysis.ipynb` | Main analysis notebook — data loading, cleaning, funding trends, institutional/geographic analysis, PI analysis, topic modeling (LDA and BERTopic), per-division breakdowns, growth/decline charts, and PDF export. |
| `NSF_21_25_SG.xlsx` | Source dataset: NSF CSE award records, 2021–2025 (one row per grant). |
| `NSF_Funding_Analysis_plots.pdf` | Exported figures from the notebook (all charts and maps in one PDF). |

## Data

The dataset (`NSF_21_25_SG.xlsx`) contains one row per NSF CSE award. Key columns used:

| Column | Meaning |
|--------|---------|
| `awd_id` | Award ID |
| `amount` | Grant amount ($) |
| `year` | Award year |
| `duration_years` | Grant length in years |
| `institution` | Awardee institution |
| `state` | Institution state |
| `pi_name` | Principal investigator |
| `div_abbr` | CSE division (CCF, CNS, IIS, OAC) |
| `cfda_num` | CFDA program code |
| `pgm_ele` | Program element |
| `abstract` | Grant abstract text (used for topic modeling) |

**Processing:** award amounts and years are parsed and typed; division, state, and institution names are normalized; institutions are geocoded via an embedded campus-coordinate dictionary for campus-level maps. For topic modeling, abstracts are cleaned and filtered to those with ≥5 tokens.

## Setup

Requires **Python 3.9**. Create the environment and install dependencies:

```bash
# with conda
conda create -n ds5500 python=3.9
conda activate ds5500

# install dependencies
pip install pandas numpy openpyxl plotly nbformat \
            scikit-learn gensim bertopic nltk kaleido
```

> `kaleido` is used by Plotly to export static images to the PDF. `openpyxl` is required to read the `.xlsx` data file.

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/chenjie-gu/ds5500.git
   cd ds5500
   ```
2. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook NSF_Funding_Analysis.ipynb
   ```
3. Run all cells top to bottom. The notebook reads `NSF_21_25_SG.xlsx` from the repo root and regenerates every figure. The final cell exports the figures to `NSF_Funding_Analysis_plots.pdf`.

## Methods

- **Funding & concentration analysis** — aggregations by year, division, institution, state, and PI; choropleth maps with true campus-level points for all institutions.
- **Topic modeling** — LDA (number of topics tuned via coherence) and BERTopic (embedding-based clustering) over the cleaned abstracts, both overall and within each division, to characterize the research subfields NSF is funding.

## Limitations

- 2025 may be partial — late-posting awards can appear after data extraction, which may overstate the late-window decline.
- Topic labels are interpretive; LDA and BERTopic can disagree on smaller, boundary topics.
- Obligated dollars are not the same as spending; multi-year awards are booked in their start year.
- Institution and subfield boundaries depend on name normalization and the abstract token filter.

## Author

Chenjie Gu — DS 5500 Capstone
