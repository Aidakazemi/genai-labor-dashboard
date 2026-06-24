# GenAI × Labor Market — Research Dashboard

> **Live dashboard →** `https://<your-username>.github.io/genai-labor-dashboard/`  
> Auto-updated every Monday via GitHub Actions from BLS, FRED, and public research data.

---

## What this is

A self-updating empirical dashboard exploring five research questions on how **Generative AI is reshaping the U.S. labor market**. Built for economists and researchers who want reproducible, data-driven analysis — not vibes.

The pipeline pulls public data weekly, commits it to this repo, and redeploys the GitHub Pages dashboard automatically. Zero servers, zero cost.

---

## Five Research Questions

| # | Question | Key Data | Method |
|---|----------|----------|--------|
| **Q1** | Which occupations face the highest GenAI exposure, and how many workers does that put at risk? | BLS OEWS 2024 + Eloundou et al. (2023) AI exposure index | Bubble scatter, exposure distribution |
| **Q2** | Are tech-sector layoffs concentrated in AI-substitutable roles, and are they accelerating post-ChatGPT? | Layoffs.fyi + BLS JOLTS | Pre/post Nov 2022 event study |
| **Q3** | Is GenAI accelerating wage polarization — widening gaps between AI-complementary and AI-substitutable roles? | BLS OEWS wage series | Wage-at-risk decomposition |
| **Q4** | Is the aggregate labor market softening in ways consistent with AI-driven demand destruction? | BLS JOLTS + CPS | JOLTS flows, Beveridge curve proxy |
| **Q5** | Is AI investment translating into job creation, or is it primarily a capital-for-labor substitution? | Stanford HAI AI Index + BLS CES | Investment-to-employment efficiency ratio |

---

## Data Sources

| Source | Data | Frequency | API |
|--------|------|-----------|-----|
| [BLS OEWS](https://www.bls.gov/oes/) | Occupational employment & wages | Annual (May) | `api.bls.gov/publicAPI/v2/` |
| [BLS CPS](https://www.bls.gov/cps/) | Unemployment by education/occupation | Monthly | BLS API |
| [BLS CES](https://www.bls.gov/ces/) | Sector employment | Monthly | BLS API |
| [BLS JOLTS](https://www.bls.gov/jlt/) | Job openings, layoffs, quits | Monthly | BLS API |
| [FRED](https://fred.stlouisfed.org/) | Indeed postings index, ECI wages, productivity | Monthly/quarterly | FRED API |
| [Layoffs.fyi (datasets/tech-layoffs)](https://github.com/datasets/tech-layoffs) | Tech layoff events | Near real-time | GitHub raw CSV |
| [Eloundou et al. 2023](https://arxiv.org/abs/2303.10130) | AI exposure scores by occupation | Static (2023) | Embedded |
| [Stanford HAI AI Index 2025](https://aiindex.stanford.edu/) | Global AI investment | Annual | Embedded |

---

## Repository Structure

```
genai-labor-dashboard/
├── .github/
│   └── workflows/
│       └── refresh.yml          # GitHub Actions: weekly data fetch + Pages deploy
├── scripts/
│   └── fetch_data.py            # Data pipeline (BLS API, FRED, layoffs.fyi, static indices)
├── data/
│   ├── raw/                     # Raw API responses (JSON)
│   └── processed/               # Clean CSVs the dashboard reads
│       ├── bls_timeseries.csv
│       ├── fred_timeseries.csv
│       ├── layoffs.csv
│       ├── ai_exposure_occupations.csv
│       ├── ai_investment.csv
│       └── manifest.json        # Last-updated timestamp
├── docs/
│   ├── index.html               # Dashboard (GitHub Pages)
│   └── data/                    # Symlinked from data/processed/ at deploy time
└── README.md
```

---

## Setup (one-time, ~5 minutes)

### 1. Fork / create this repo

```bash
git clone https://github.com/<you>/genai-labor-dashboard
cd genai-labor-dashboard
```

### 2. Enable GitHub Pages

`Settings → Pages → Source: GitHub Actions`

### 3. (Optional) Add free API keys for higher rate limits

```
Settings → Secrets and variables → Actions → New repository secret
```

| Secret | How to get | Impact |
|--------|------------|--------|
| `BLS_API_KEY` | [bls.gov/developers/](https://www.bls.gov/developers/) — free signup | 500 req/day vs 25/day without key |
| `FRED_API_KEY` | [fred.stlouisfed.org/docs/api/](https://fred.stlouisfed.org/docs/api/api_key.html) — free signup | Required for some series |

### 4. Trigger the first run

`Actions → Refresh Data & Deploy Dashboard → Run workflow`

Your dashboard will be live at `https://<username>.github.io/genai-labor-dashboard/`

---

## Running locally

```bash
python scripts/fetch_data.py          # pull data
cd docs && python -m http.server 8080 # serve dashboard
# open http://localhost:8080
```

---

## Methodology Notes

**AI Exposure Index:** This dashboard uses occupation-level exposure scores from Eloundou et al. (2023) "GPTs are GPTs: An Early Look at Labor Market Exposure to Large Language Models." Scores reflect the proportion of tasks in an occupation that could be meaningfully affected by GPT-class models. This is **not** a prediction of job loss — exposure captures automation *potential*, not actual displacement outcomes.

**Causal identification:** Disentangling AI effects from macro cycles (2020–2021 over-hiring, 2022 rate hikes) is genuinely hard. This dashboard is **descriptive**, not causal. For causal work, see Acemoglu (2024), Autor et al. (2022), and Brynjolfsson et al. (2023).

**Data freshness:** BLS JOLTS is ~6 weeks lagged; OEWS is annual (May release). The dashboard shows the most recent available data with the last-updated timestamp displayed prominently.

---

## Key References

- Eloundou, T., Manning, S., Mishkin, P., & Rock, D. (2023). *GPTs are GPTs: An Early Look at Labor Market Exposure to Large Language Models.* [arXiv:2303.10130](https://arxiv.org/abs/2303.10130)
- Acemoglu, D. (2024). *The Simple Macroeconomics of AI.* NBER Working Paper 32487.
- Autor, D., Levy, F., & Murnane, R. J. (2003). *The Skill Content of Recent Technological Change.* QJE.
- Stanford HAI. (2025). *AI Index Report 2025.* Stanford University.
- Brynjolfsson, E., Li, D., & Raymond, L. (2023). *Generative AI at Work.* NBER Working Paper 31161.

---

## License

Data from BLS, FRED, and Stanford HAI is public domain / openly licensed. Original code in this repo is MIT licensed.

---

*Built by [your name] · PhD researcher in Business Analytics · [LinkedIn](#) · [GitHub](#)*
