# 🚀 Golden Hour- Crime Prevention and Missing Person Detection 
---

## 👥 Team

| Field | Value |
|---|---|
| **Team Name** | AlgoRhythms |
| **Track** | AI and Forensics |
| **Team Lead** | Shreeya Sarasvadia |
| **Members** | Jil, Siddhant, Krishn |

---

# Golden Hour Grid

*One map to prevent the next case and find the current one.*

## 🎯 Problem Statement

Police patrols follow fixed routes even as crime patterns shift with time, trends and local events. A missing-person case starts as scattered tips, CCTV notes and paperwork, with nothing linking the search to the risks in the area. Station House Officers (SHOs) lose hours and patrol effort as a result. The hackathon brief cites 80,000+ missing children a year in India (NCRB 2022).

---

## 💡 Solution

Golden Hour Grid puts crime prevention and missing-person search on one map of a 10×10 grid of ~400 m cells. A prevention engine ranks the top 5 at-risk zones for the week. A search engine checks each tip and CCTV sighting for time-and-distance feasibility and builds a probability grid that grows with time. A fusion engine combines the two (`priority = search × (1 + 0.5 × risk)`) to allocate patrol beats, then drafts the SHO brief, public appeal and case file. Code computes every number, and the AI layer only writes and explains.

---

## ✨ Key Features

- **Predictive hotspots:** Top-5 weekly risk zones scored on crime severity, recency decay (14-day half-life), day-of-week and evening/night patterns, and festival/event spikes, each with a plain-language rationale and trend.
- **Backtesting:** Compares the model's top-5 hit rate against a "same-as-last-week" baseline.
- **Lead feasibility and ranking:** Tips and CCTV sightings are turned into structured leads. Impossible or contradictory leads (for example, a distance that can't be covered in the elapsed time by foot, cycle or bus) are flagged and down-ranked, and each lead gets a recommended action.
- **Growing search area:** A probability grid over the district that widens with hours since last seen, re-centres on the latest credible sighting and boosts transit nodes. An interactive time slider drives it.
- **Fused patrol allocation:** Risk and search layers combine into a priority score, and N patrol units are assigned to cells with consecutive time windows that shrink as the search area widens.
- **One-click documents:** DOCX SHO patrol brief, public appeal (with home address and phone numbers deliberately excluded) and internal case file.
- **AI with a safe fallback:** Narrative text uses an optional OpenAI-compatible model and falls back to deterministic templates, so documents always generate.

---

## 🛠️ Tech Stack

| Category | Technologies |
|---|---|
| **Languages** | Python |
| **Frameworks** | Streamlit, Folium (`streamlit-folium`) |
| **IBM Technologies** | IBM Bob (AI development partner). [Add watsonx.ai here only if you used it] |
| **Databases** | None (CSV and JSON files) |
| **Other** | python-docx, pandas, numpy, optional OpenAI-compatible LLM API |

---

## 📁 Repository Structure

```
├── src/                  # All source code
│   ├── app.py            # Streamlit UI (Prevention, Search, SHO Brief tabs)
│   ├── hotspot.py        # Risk scoring, top-5 prediction, backtest
│   ├── search.py         # Lead parsing, feasibility, probability grid, ranking
│   ├── fusion.py         # Layer fusion and beat allocation
│   ├── llm.py            # Narrative generation with template fallback
│   ├── docs.py           # DOCX generation (brief, appeal, case file)
│   ├── mockdata.py       # Synthetic data generator
│   └── requirements.txt
├── docs/                 # Written documentation
│   ├── problem-statement.md
│   ├── solution-overview.md
│   ├── architecture.md
│   └── setup-guide.md
├── demo/                 # Demo artifacts
│   ├── screenshots/      # App screenshots
│   └── demo-video-link.txt  # Link to demo video
├── presentation/         # Slide deck
└── submission.yaml       # Structured submission metadata
```

---

## ⚡ How to Run

```bash
# 1. Clone the repo
git clone https://github.com/[your-repo].git
cd [your-repo]/src

# 2. Install dependencies (Python 3.10+)
pip install -r requirements.txt

# 3. (Optional) enable AI-written narrative text
pip install openai
export OPENAI_API_KEY=your_key      # optional; templates are used if unset
# export OPENAI_BASE_URL=...        # optional, for compatible services
# export OPENAI_MODEL=gpt-4o-mini   # optional

# 4. Generate the synthetic dataset (creates data/crimes.csv, events.csv, case.json)
python mockdata.py

# 5. Run the app
streamlit run app.py

# 6. (Optional) generate the DOCX documents into outputs/
python docs.py
```

Run all commands from inside `src/` because the code reads and writes `data/` and `outputs/` relative to the working directory.

---

## 🖥️ Demo

| Artifact | Link |
|---|---|
| 📹 Demo Video | [See demo/demo-video-link.txt](demo/demo-video-link.txt) |
| 🌐 Live Demo | [See demo/live-demo-url.txt](demo/live-demo-url.txt) |
| 🖼️ Screenshots | [See demo/screenshots/](demo/screenshots/) |
| 📊 Presentation | [See presentation/slides.pdf](presentation/) |

---

## ⚠️ Known Limitations

- **Synthetic data only:** The district ("Narmgarh"), crime records and the missing-person case are generated. The results show how the method behaves, not real-world accuracy.
- **Backtest is lenient:** A "hit" means at least one top-5 cell had an incident that week. With incidents spread across a 100-cell grid, both the model and the baseline score high, so it doesn't yet prove a clear advantage.
- **Tip parsing is keyword-based:** Locations and times are matched from a fixed list of known places and phrases, and sighting times are rounded to the hour. It is not a real language-understanding pipeline, and tips in other wording or in Hindi are not handled.
- **DOCX generation is not wired into the UI:** Documents are produced by running `python docs.py`, not by a button in the app.
- **Probabilities are heuristic:** The search grid and priority weights are hand-tuned and not calibrated against real cases.
- **Prevention analysis week:** The demo predicts the last week in the dataset using only earlier history, rather than a genuinely future week.

---

## 🏅 What We're Most Proud Of

Fusing two normally separate workflows on one map, so a cell that is both a likely place to find the missing person and a high-risk area is prioritised for patrols. We are also proud that every score is explainable: a feasibility check flags impossible tips, each hotspot comes with reasons, and the AI layer never invents numbers, it only explains what the code computed.
