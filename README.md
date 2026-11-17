# How Good Are LLMs at 20 Questions?

An evaluation of large language models playing the game **20 Questions**: in both roles, across model pairings, with metrics that go beyond win rate.

> **Status:** Proposal submitted (Nov 2026) · Pilot complete · Full experiment in progress
> Course project for CSE 519 (Data Science Fundamentals), Stony Brook University.

---

## The question

20 Questions is a small, controlled test of several things LLMs are supposed to be good at: forming hypotheses, asking informative questions, tracking state over many turns, and answering consistently about a hidden fact.

We study it from **both sides of the table**:

| Role | What the model does | What we're testing |
|---|---|---|
| **Answerer** | Picks a secret word and answers yes/no questions about it | Honesty and consistency over many turns; how random its word choice really is |
| **Guesser** | Asks up to 20 yes/no questions, then guesses the word | Strategy, information gain per question, efficiency |

> **Terminology:** we use *Answerer* (holds the secret word) and *Guesser* (asks the questions) everywhere in the code and writeup.

## Work so far

### Pilot experiment
A small test run to validate the game pipeline and get early numbers.

- **Models:** `<MODEL_A>` and `<MODEL_B>`, each playing both roles
- **Games:** `<N>` games, every game fully logged (questions, answers, final guess, outcome)
- **Notebook:** [`notebooks/20q_pilot.ipynb`](notebooks/20q_pilot.ipynb) ([Open in Colab](<COLAB_LINK>))

| Guesser ↓ / Answerer → | `<MODEL_A>` | `<MODEL_B>` |
|---|---|---|
| `<MODEL_A>` | `<win %>` (avg `<q>` questions) | `<win %>` (avg `<q>` questions) |
| `<MODEL_B>` | `<win %>` (avg `<q>` questions) | `<win %>` (avg `<q>` questions) |

![Pilot results](results/figures/pilot_win_rate.png)

**Early observations:** `<2–3 bullets: e.g. where Guessers stalled, any Answerer inconsistencies spotted>`

### Proposal
The full project proposal (problem, background, pilot, approach, metrics) is in [`docs/proposal.pdf`](docs/proposal.pdf).

## Planned approach

- **At least 4 models**, played in every pairing, with each model taking both roles
- `<K>` games per pairing for stable estimates
- All games saved as structured logs so every metric can be recomputed from raw data

## Metrics

| Metric | What it measures |
|---|---|
| **Win rate** | Share of games where the Guesser names the word within 20 questions |
| **Questions to win** | Efficiency of successful games |
| **Question quality** | How close each question comes to splitting the remaining possibilities in half (information gain) |
| **Answerer honesty** | Whether answers stay consistent with the secret word across the whole game |
| **Secret-word randomness** | How the Answerer's word choices compare with a true random sample from the same word list |

## Repository layout

```
.
├── README.md
├── notebooks/
│   └── 20q_pilot.ipynb        # pilot experiment (runs in Colab)
├── src/                       # game engine, model wrappers, metrics (full experiment)
├── data/
│   └── pilot_games.jsonl      # raw logs, one game per line
├── results/
│   └── figures/               # plots used in the writeup
├── docs/
│   └── proposal.pdf
├── requirements.txt
└── .env.example               # API key names; real keys never committed
```

## Running it

**Colab (easiest):** open the notebook via the badge above, add your API keys under *Secrets* (🔑 in the left sidebar), and run all cells.

**Locally:**
```bash
git clone <REPO_URL>
cd llm-20-questions
pip install -r requirements.txt
cp .env.example .env    # then fill in your API keys
jupyter notebook notebooks/20q_pilot.ipynb
```

## Related work

Includes the Kaggle *LLM 20 Questions* competition and prior research on LLM question-asking and multi-turn reasoning. See the Background section and References in the [proposal](docs/proposal.pdf).

## Roadmap

- [x] Game pipeline (Answerer / Guesser loop with full logging)
- [x] Pilot: 2 models, both roles
- [x] Project proposal
- [ ] Scale to 4+ models, all pairings
- [ ] Implement question-quality and honesty metrics
- [ ] Secret-word randomness analysis
- [ ] Progress report
- [ ] Final report

## Authors

`<Maaz Khan>` · `<Adriel ...>`