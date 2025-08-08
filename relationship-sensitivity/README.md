# Repo Layout
relationship-sensitivity/
├─ src/rel_sense/
│  ├─ __init__.py
│  ├─ scoring.py          # CLI: totals + radars + Excel breakdown
│  ├─ radar.py            # radar chart helper
│  └─ sensitivity.py      # 1D/3D weight sweeps
│  └─ run_sensitivity.py      # Driver and CLI
├─ examples/
│  └─ relationship.csv
├─ app.py                 # Streamlit demo (optional)
├─ requirements.txt
├─ README.md
├─ LICENSE
├─ .gitignore
└─ .github/workflows/ci.yml

# 💞 Relationship Sensitivity

Ever wondered *why* you feel more connected to one partner than another — and how much that feeling depends on certain qualities?  
**Relationship Sensitivity** is a small but mighty toolkit for quantifying and exploring the dynamics between you and the important people in your life.

At its core, it takes a set of **relationship dimensions** (e.g., *Emotional connection*, *Shared values & life goals*, *Aesthetics & taste*, etc.) and assigns each a **weight** representing how important that dimension is to you.  
It then scores two people (or more, if you extend it) across those dimensions — for example, a current partner vs. an ex — and lets you:

- 📊 **Score & compare**: See where each person shines (or struggles) across dimensions.
- 📈 **Visualize**: Generate radar charts and sensitivity plots to understand the strengths and gaps.
- 🧪 **Run sensitivity analysis**: Test *"what-if"* scenarios by tweaking how much weight you give to certain traits, revealing:
  - **1D analysis** – How much you’d need to change *just one dimension’s* importance before the “winner” flips.
  - **3D analysis** – How combinations of three dimensions influence the outcome at once.


## Features
- Weighted totals (Option A vs B)  
- Radar charts (unweighted / weighted)  
- 1D & 3D **weight sensitivity** (find boundary where two options tie)  
- CSV/Excel outputs for your own charts

# Quickstart


## 1) Install
```bash
pip install -r requirements.txt
```

## 2) Prepare your data (CSV)
Columns (case-sensitive):
- Dimension – factor name
- Weight – importance (0–1). If not summing to 1, we normalize.
- ExScore or Ex_score_raw – score for Option A (e.g., “Ex”)
- CurScore or Cur_score_raw – score for Option B (e.g., “Current”)

Example (examples/relationship.csv):
```
Dimension,Description,Weight,Ex_score_raw,Cur_score_raw
Emotional connection,Years of shared history,0.18,10,7
Unconditional acceptance,Accept flaws and vulnerability,0.14,9,8
Care & daily attentiveness,Daily care and thoughtfulness,0.11,8,9
Shared values & life goals,Life goals alignment,0.14,6,9
Income & economic potential,Financial potential,0.11,6,9
Activity & interest fit,Shared hobbies,0.08,6,9
Physical preference,Height/Looks,0.04,9,7
Communication & conflict,Conflict resolution,0.07,8,9
Patience & holding space,Willingness to wait,0.04,7,9
Social handling,Handling public pressure,0.02,7,8
Aesthetics & taste,Love for beauty,0.10,6,9
```
# Use app.py
```
python3 -m streamlit run app.py
``` 

# If You Chose to Run Everything in Your Ternimal

## 3) Run Scoring and Plot Radar Chart
Calculate and export scores for current and ex-partners.
```bash
python3 -m src.rel_sense.scoring examples/relationship.csv
```
Output:
outputs/scores.csv — Detailed scores per dimension.

**Plot Radar Chart**
```
python3 -m src.rel_sense.plots --radar outputs/scores.csv
```

## 4) Sensitivity Analysis 

### 4.1 Run 1D sensitivity
Vary one dimension’s weight to find the decision boundary.
```
python3 -m src.rel_sense.run_sensitivity examples/relationship.csv \
  --dim "Emotional connection"
```
Output:
outputs/boundary_emotionalconnection.csv

### 4.3 Run 3D sensitivity
```bash
python3 -m src.rel_sense.run_sensitivity examples/relationship.csv \
  --tri "Emotional connection,Shared values & life goals,Aesthetics & taste"
```
### 4.4) Plot 1D and 3D sensitivity boundary
```
python3 -m src.rel_sense.plots --boundary outputs/boundary_emotionalconnection.csv
python3 -m src.rel_sense.plots --tri outputs/tri_sensitivity.csv --eps 0.02
```


Lastly, find Results in Output Folder


## Common issues

- ImportError: attempted relative import with no known parent package

    Run with -m:
    ```python3 -m src.rel_sense.scoring examples/relationship.csv
    python3 -m src.rel_sense.run_sensitivity examples/relationship.csv
    ```

- “Dimension 'X' not found”

    Use --list to see exact names, then pass them with quotes:
    ```--dim "Emotional connection" \
    --tri "Emotional connection,Shared values & life goals,Aesthetics & taste"
    ```
- Weights don’t sum to 1

    We auto-normalize and print a warning.


### License
MIT - do what you want, no warranty.