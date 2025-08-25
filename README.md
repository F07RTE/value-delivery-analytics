# Value Delivery Analytics

Analyze, visualize, and generate insights from value delivery records (CSV) with an optional GPT-powered analysis step. Outputs are saved as timestamped text reports in the `results/` folder.

## Overview
This project centers around a Jupyter Notebook (`value_deliveries_analysis.ipynb`) that:
- Loads and explores value delivery data from `value_deliveries.csv`
- Creates visualizations and summaries with pandas/Matplotlib/Seaborn
- Optionally uses the OpenAI API to generate narrative analyses (customer- and marketing-focused)
- Writes analysis reports to `results/` as timestamped `.txt` files

Example results (already present):
- `results/customer_analysis_Acme_Corp_YYYYMMDD_HHMMSS.txt`
- `results/marketing_analysis_Acme_Corp_YYYYMMDD_HHMMSS.txt`

## Tech Stack
- Python 3.10.x
- Jupyter Notebook (run via VS Code or JupyterLab)
- pandas, numpy, matplotlib, seaborn
- OpenAI (optional; reads `OPENAI_API_KEY` from environment)
- python-dotenv for local `.env` loading

All dependencies are pinned in `requirements.txt`.

## Project Structure
```
value-delivery-analytics/
├── value_deliveries_analysis.ipynb   # Main analysis notebook
├── value_deliveries.csv              # Input dataset
├── results/                          # Generated text reports (git-ignored in README examples)
├── requirements.txt                  # Pinned Python dependencies
├── .env                              # Local secrets (ignored)
├── .gitignore                        # Ignores venv, .env, results
└── README.md                         # This file
```

### Dataset
`value_deliveries.csv` columns:
- Delivery
- Technologies
- Value Type
- Customer
- Delivery Date
- Contributors

## Getting Started

### 1) Create and activate a virtual environment
You can use your existing `value-delivery-analytics-env` or create a new one (recommended per-user).

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2) Install dependencies
```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3) Configure environment variables (OpenAI)
Option A — `.env` file (recommended for local dev; do not commit):
```
OPENAI_API_KEY=YOUR_KEY_HERE
```
The notebook loads this if you include at the top:
```python
from dotenv import load_dotenv
load_dotenv()  # loads variables from .env in the project root
```

Option B — macOS Keychain (no plaintext in files):
- Store the key:
```bash
security add-generic-password -a "$USER" -s OPENAI_API_KEY -U
```
- Load it automatically in each shell by adding to `~/.zshrc`:
```bash
export OPENAI_API_KEY="$(security find-generic-password -a \"$USER\" -s OPENAI_API_KEY -w 2>/dev/null || true)"
```
- Apply it now:
```bash
source ~/.zshrc
```

Verify the variable is set (without printing it):
```bash
python -c "import os; print('OPENAI_API_KEY set:', bool(os.getenv('OPENAI_API_KEY')))"
```

### 4) Run the notebook
- In VS Code: open the repo folder, open `value_deliveries_analysis.ipynb`, select your venv kernel, run cells.
- Or with JupyterLab (optional install):
```bash
pip install jupyterlab
jupyter lab
```
Open `value_deliveries_analysis.ipynb` in the UI.

## Outputs
- Generated narrative reports are written to `results/`.
- Filenames follow a pattern like `customer_analysis_<Customer>_<YYYYMMDD>_<HHMMSS>.txt` and `marketing_analysis_<Customer>_<YYYYMMDD>_<HHMMSS>.txt`.

## Reproducibility & Dependencies
- Dependencies are pinned in `requirements.txt`.
- If you add/upgrade packages:
```bash
pip install <pkg>
pip freeze > requirements.txt
```
- Optional: use `pip-tools` for lock-style workflows:
```bash
pip install pip-tools
# Put your top-level deps in requirements.in, then:
pip-compile --generate-hashes
pip-sync requirements.txt
```

## Security & Secrets
- Do not commit `.env` or any API keys.
- `.gitignore` already excludes: your virtual environment directory, `results/`, and `.env`.
- Consider adding secret scanning pre-commit hooks (e.g., `detect-secrets`, `truffleHog`) and `nbstripout` to strip notebook outputs on commit.

## Troubleshooting
- OPENAI_API_KEY not detected in the notebook
  - Ensure `load_dotenv()` is called before using the API, or export the variable in your shell.
- Kernel mismatch or missing packages
  - Confirm you’re using the correct venv kernel in VS Code/Jupyter; reinstall with `pip install -r requirements.txt`.
- Apple Silicon build issues
  - Install Xcode Command Line Tools: `xcode-select --install` and retry.

## License
Add your preferred license (e.g., MIT) as `LICENSE` in the repo root.
