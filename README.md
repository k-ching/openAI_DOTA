# Behavioral Transparency in Multi-Agent RL

**Strategic Decoupling and the Economic Optimization of Tactical Shocks**

Kenny Ching — School of Business, University of Auckland  
`kenny.ching@auckland.ac.nz`

---

> **Abstract.** Utilizing high-fidelity telemetry from OpenAI Five in the imperfect-information environment of *Dota 2*, we provide empirical transparency into how reinforcement learning agents process localized tactical shocks. We demonstrate that average human cohorts exhibit intense tactical myopia while the RL agent executes perfect strategic decoupling. Crucially, when compared exclusively against apex human experts in identical economic states, the statistical divergence between biological and silicon agents largely vanishes — revealing that RL systems do not invent incomprehensible strategies, but mathematically converge on the optimization phenotype of world-champion human players.

**Journal:** *Nature Machine Intelligence* (under review)  
**Keywords:** Machine behavior · Multi-agent reinforcement learning · Behavioral transparency · Strategic decoupling · Econometrics · Dota 2

---

## Repository Structure

```
.
├── README.md
├── data/
│   └── tf_events_master_unified_v2.csv   # Master econometric dataset (9,228 rows)
├── notebooks/
│   ├── step1_data_acquisition.ipynb      # Download TI data + parse .dem replays
│   ├── step2_file_prep.ipynb             # Feature extraction & dataset construction
│   └── step3_analysis.ipynb             # Econometric analysis & figure generation
├── figures/
│   ├── fig_negative_distribution.png
│   ├── fig_negative_efficiency.png
│   ├── fig_positive_distribution.png
│   ├── fig_positive_efficiency.png
│   ├── fig_econometric_convergence.png
│   ├── fig_tost_equivalence.png
│   └── fig_k_robustness.png
└── paper/
    ├── sn-article.tex
    └── sn-bibliography.bib
```

---

## Data Sources

### Human Baseline — TI8 and TI9 (OpenDota API)

The human control group consists of Main Event matches from The International 2018 (TI8, league ID 9870) and The International 2019 (TI9, league ID 10749), retrieved via the [OpenDota public API](https://api.opendota.com). These tournaments represent the apex of human competitive performance during the exact patch era in which OpenAI Five was trained.

- **9,080 human tactical shock observations** across both tournaments
- No API key required; public tier rate limit applies (60 req/min)

### AI Treatment — OpenAI Five Replay Files

The three `.dem` replay files from the April 2019 OpenAI Five Finals are available from the official OpenAI blog post:

**→ https://openai.com/index/openai-five-defeats-dota-2-world-champions/**

| File | Match | Duration | AI Side | Result |
|---|---|---|---|---|
| `og_game_1_04132013569395266.dem` | OG vs OpenAI Five — Game 1 | 39.8 min | Dire | OG wins |
| `og_game2_04132030946085141.dem` | OG vs OpenAI Five — Game 2 | 22.4 min | Radiant | OpenAI wins |
| `coop_game_04132076146922985.dem` | OpenAI Five Coop Exhibition | 60.4 min | Both | Radiant wins |

> **Note:** Raw `.dem` files are the property of Valve Corporation and are not redistributed in this repository. Download them directly from the OpenAI link above.

These files were parsed using [Clarity](https://github.com/skadistats/clarity-examples), an open-source Source 2 replay parser, to produce millisecond-precision combat logs. The parsing pipeline is fully documented in `step1_data_acquisition.ipynb`.

---

## Dataset

**`data/tf_events_master_unified_v2.csv`** — 9,228 rows × 10 columns

| Column | Type | Description |
|---|---|---|
| `match_id` | int | Match identifier |
| `shock_type` | str | `negative` (net kill deficit) or `positive` (net kill surplus) |
| `is_ai` | int | 1 = OpenAI Five, 0 = human |
| `did_win` | int | 1 = focal team won the match |
| `fight_start` | int | Game clock time (seconds) at teamfight start |
| `current_gold_lead` | int | Focal team net worth advantage at fight start (gold) |
| `gadv_d30` | int | Change in gold advantage at t + 30s |
| `gadv_d60` | int | Change in gold advantage at t + 60s |
| `gadv_d120` | int | Change in gold advantage at t + 120s |
| `gadv_d180` | int | Change in gold advantage at t + 180s |

**Summary statistics:**

| Cohort | Rows | Negative shocks | Positive shocks |
|---|---|---|---|
| OpenAI Five (AI) | 148 | 61 | 87 |
| Human (TI8 + TI9) | 9,080 | 4,553 | 4,527 |
| **Total** | **9,228** | **4,614** | **4,614** |

---

## Reproduction Pipeline

All notebooks are designed to run in **Google Colab** with no local setup required.

### Step 1 — Data Acquisition (`step1_data_acquisition.ipynb`)

Three parts in one notebook:

**Part A:** Downloads TI8 + TI9 match telemetry from the OpenDota API (~15–20 min). Saves raw JSONs to `/content/data/raw/`. Resumes automatically if interrupted.

**Part B:** Upload the three `.dem` files downloaded from the OpenAI blog post above.

**Part C:** Clones Clarity, builds the combatlog parser (Java 21, ~2 min first run), and parses all three `.dem` files into structured combat logs. Outputs are verified against expected line counts.

### Step 2 — Feature Extraction (`step2_file_prep.ipynb`)

Parses the Clarity combat logs into teamfight events and gold timelines for the AI cohort. Merges with the human baseline (read from disk, fetched from API, or uploaded from existing CSV — three options documented in the notebook). Outputs `tf_events_master_unified_v2.csv`.

### Step 3 — Analysis & Figures (`step3_analysis.ipynb`)

Uploads the master CSV and reproduces all tables and figures in the manuscript:

- **Tables 1 & 2:** OLS regressions with HC1 heteroskedasticity-robust standard errors and Cohen's *d* effect sizes
- **Figures 1–4:** KDE distributional plots and conversion efficiency bar charts for negative and positive shocks
- **Figure 5:** Econometric Convergence Map — temporal decay of β_AI across biological baselines
- **Figure 6:** TOST equivalence tests (bound = ±500 gold)
- **Figure 7:** k-robustness check across k ∈ {5, 10, 15} nearest neighbours

All seven figures are downloaded as a zip at the end of the notebook.

---

## Key Methodological Notes

- **Teamfight identification:** Combat log events clustered with a 20-second gap threshold
- **Gold timeline resolution:** 60-second intervals (matching OpenDota's `radiant_gold_adv` array)
- **Buyback detection:** Two consecutive `looses` gold events within 0.5s for the same hero = one buyback (the game engine misspells "loses" as "looses" — this is a known Dota 2 engine quirk, not a parsing error)
- **State-space matching:** k = 5 nearest neighbours on standardised game clock time and current gold lead
- **Standard errors:** HC1 heteroskedasticity-robust throughout
- **Equivalence testing:** TOST with ±500 gold bound (~3% of typical late-game net worth)

---

## Dependencies

All notebooks install dependencies automatically via `pip`. For local use:

```bash
pip install pandas numpy statsmodels scikit-learn matplotlib seaborn scipy requests tqdm
```

Parsing `.dem` files additionally requires:
- Java 21+
- Git (to clone Clarity)

---

## Citation

```bibtex
@article{ching2026behavioral,
  title     = {Behavioral Transparency in Multi-Agent RL: Strategic Decoupling
               and the Economic Optimization of Tactical Shocks},
  author    = {Ching, Kenny},
  journal   = {Nature Machine Intelligence},
  year      = {2026},
  note      = {Under review}
}
```

---

## License

Code in this repository is released under the **MIT License**.  
The processed dataset (`tf_events_master_unified_v2.csv`) is released under **CC BY 4.0**.  
Raw `.dem` replay files are the property of Valve Corporation and are subject to their terms of service.
