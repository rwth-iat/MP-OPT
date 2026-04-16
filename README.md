# MP-OPT

`MP-OPT` is a notebook-first companion repository for the paper *Optimal Executable Process Plan Generation for Capability-Constrained Modular Plants*.

This repository is intentionally lightweight. Instead of packaging the project as an installable library, it keeps the workflow close to the original research setup:

- one main planning notebook,
- one batch runner notebook,
- the small set of Python helper scripts those notebooks depend on,
- two committed example artifacts for the paper-facing feasible and infeasible cases.

## Primary Entry Points

- [MP-OPT.ipynb](MP-OPT.ipynb)
  Main notebook for the modular-plant BFS + optimization workflow.
- [batch_runner.ipynb](batch_runner.ipynb)
  Notebook used to run many seeds in batch mode by extracting code from `MP-OPT.ipynb`.

## Included Files

- `MP-OPT.ipynb`
- `batch_runner.ipynb`
- `supporting_scripts/ModPlant_Flow_Generator.py`
- `supporting_scripts/ModPlant_Flow_To_General_Recipe.py`
- `supporting_scripts/ModPlant_General_Recipe_To_Json.py`
- `supporting_scripts/ModPlant_Reaction_Rules.py`
- `supporting_scripts/ModPlant_Render_Tools.py`
- `artifacts/reference_case/`
- `artifacts/infeasible_seed_100066/`

The repository keeps the main notebooks in the root, while the `ModPlant_*.py` helper modules are grouped under `supporting_scripts/` to keep the top level cleaner.

Runtime output folders are pre-created in the repository root for convenience:

- `Log/`
- `Result/Feasible/`
- `Result/Unfeasible/`
- `Recipe/Json/`
- `Recipe/XML/`

## Setup

Install the notebook dependencies into your current Python environment:

```bash
python -m pip install -r requirements.txt
```

Install GLPK separately so Pyomo can use the default open-source solver:

- macOS (Homebrew): `brew install glpk`
- Ubuntu/Debian: `sudo apt-get install glpk-utils`

The notebooks in this repository default to `glpk`.

Optional:

- `gurobipy` for Gurobi-based solving if you want better performance.
- Gurobi requires you to obtain and configure your own license before use.

## Usage

1. Open Jupyter from the repository root.
2. Start with [MP-OPT.ipynb](MP-OPT.ipynb) for the main workflow.
3. Use [batch_runner.ipynb](batch_runner.ipynb) for seed sweeps. It reads `MP-OPT.ipynb` directly from the repository root.
4. Keep the helper `.py` files inside `supporting_scripts/`; the notebooks import them from there automatically.

By default, the optimization cells use `glpk`. If you have a licensed Gurobi installation and want faster solves, you can switch the solver name in the notebook cells from `glpk` to `gurobi`.

The repository keeps two committed paper-related outputs:

- [artifacts/reference_case/](artifacts/reference_case)
  Reference feasible case, including the raw selected trace and a paper-aligned grouped trace.
- [artifacts/infeasible_seed_100066/](artifacts/infeasible_seed_100066)
  Early rejection example for the missing `200 rpm` stirring capability case.
