# ModPlant-OPT

Notebook companion repository for the paper **Optimal Executable Process Plan Generation for Capability-Constrained Modular Plants** by Bowen Chen, Michael Winter, Torben Miny, and Tobias Kleinert.

`ModPlant-OPT` is a lightweight research artifact that stays close to the original experimental workflow used in the paper. The repository provides the main planning notebook, the batch runner notebook, the small set of helper scripts they depend on, and committed example artifacts for the representative feasible and infeasible cases discussed in the manuscript.

## Paper Scope

The paper studies how an ISA-88 General Recipe and a fixed modular plant configuration can be turned into an optimized executable process plan under capability, interface, capacity, and recipe-progress constraints. In the repository, this workflow is represented by:

- recipe generation and parsing utilities,
- reaction-rule construction,
- finite-state legality filtering,
- reachable-state exploration with BFS,
- selection of one preferred plan by optimization.

This repository is a companion artifact for the paper, not a packaged industrial software product.

## What This Repository Reproduces

This public release focuses on the two paper-facing cases that are most important for understanding and reproducing the contribution:

- **Reference feasible case**  
  A representative four-module HC10-HC40 planning case that yields a feasible executable plan. The committed artifact includes a paper-aligned grouped trace with **17 published steps**.

- **Reference infeasible case**  
  A deterministic capability-mismatch case (`seed_100066`) that is rejected because the required **200 rpm stirring** capability is unavailable.

- **Optional batch exploration**  
  The batch notebook supports seed sweeps for exploratory runs beyond the two committed paper artifacts.

The repository intentionally ships only the minimal reproducibility material needed for these representative cases. It does not aim to publish every historical log, benchmark sweep, or internal development artifact used during the broader project.

## Repository Layout

- [ModPlant-OPT.ipynb](ModPlant-OPT.ipynb)  
  Main notebook for the modular-plant planning workflow.

- [batch_runner.ipynb](batch_runner.ipynb)  
  Batch notebook that runs multiple seeds by extracting executable code from `ModPlant-OPT.ipynb`.

- [supporting_scripts](supporting_scripts)  
  Helper modules for flow generation, General Recipe conversion, reaction-rule generation, and rendering support.

- [artifacts/reference_case](artifacts/reference_case)  
  Committed artifact bundle for the feasible reference case, including `plant.json`, `order.json`, rule and recipe exports, solver traces, and `paper_trace.json`.

- [artifacts/infeasible_seed_100066](artifacts/infeasible_seed_100066)  
  Committed artifact bundle for the capability-mismatch case, including the fallback corpus and summary output.

- `Log/`, `Result/Feasible/`, `Result/Unfeasible/`, `Recipe/Json/`, `Recipe/XML/`  
  Runtime output directories used by the notebooks.

## Requirements

Install the Python dependencies into your current environment:

```bash
python -m pip install -r requirements.txt
```

The main notebook dependencies are listed in [requirements.txt](requirements.txt) and include `pandas`, `pyomo`, `psutil`, `pyvis`, `ipython`, and `jupyterlab`.

Install GLPK separately so Pyomo can use the default open-source solver:

- macOS (Homebrew): `brew install glpk`
- Ubuntu/Debian: `sudo apt-get install glpk-utils`
- Windows: `conda install -c conda-forge glpk`
- Windows alternative: `choco install glpk`

The public notebook configuration defaults to `glpk`.

Optional:

- `gurobipy` can be used for better performance.
- Gurobi requires you to obtain and configure your own license.

## Quick Start

1. Install the Python dependencies from [requirements.txt](requirements.txt).
2. Install GLPK so that Pyomo can access the default solver.
3. Launch Jupyter from the repository root:

```bash
jupyter lab
```

4. Open [ModPlant-OPT.ipynb](ModPlant-OPT.ipynb).
5. Run the notebook cells from top to bottom.

## Reproducing the Paper Cases

### 1. Feasible Reference Case

Use [ModPlant-OPT.ipynb](ModPlant-OPT.ipynb) as the main entry point.

Expected outcome:

- a feasible planning result,
- generated recipe and reaction-rule exports,
- a selected operation trace,
- outputs written into the runtime folders,
- reference artifact counterparts available under [artifacts/reference_case](artifacts/reference_case).

The committed summary for this case is [artifacts/reference_case/summary.json](artifacts/reference_case/summary.json), which records:

- `bfs_feasible: true`
- `paper_trace_published_steps: 17`

### 2. Infeasible Capability-Mismatch Case

Use [batch_runner.ipynb](batch_runner.ipynb) if you want to execute deterministic seed-based runs. To reproduce the committed infeasible paper case, set both `START_INDEX` and `END_INDEX` to `100066` so that the batch notebook evaluates only this single seed, then run the notebook.

Expected outcome:

- early rejection before a full feasible plan is produced,
- a summary indicating capability mismatch,
- a reported missing stirring capability at `200 rpm`.

The committed summary for this case is [artifacts/infeasible_seed_100066/summary.json](artifacts/infeasible_seed_100066/summary.json), which records:

- `status: SKIPPED_CAPABILITY_MISMATCH`
- `capability_mismatch: true`
- `skip_reason: Missing required stirring RPMs: 200`

## Expected Outputs

During notebook execution, the repository writes runtime outputs into the following locations:

- `Log/` for batch logs and per-seed summaries,
- `Result/Feasible/` and `Result/Unfeasible/` for generated result corpora,
- `Recipe/XML/` and `Recipe/Json/` for exported General Recipe files.

The `artifacts/` directory contains committed example outputs that can be inspected without rerunning the notebooks.

## Solver Notes

The public version of the notebooks defaults to `glpk` so the repository can be used with an open-source solver stack.

If you have access to Gurobi and a valid license, you may switch the solver setting in the notebook cells from `glpk` to `gurobi` for faster runs. Some committed artifact files were generated during repository preparation with a different solver configuration; the current public default is still `glpk`.

## Scope and Limitations

- This repository is a paper companion artifact intended for reproducibility and inspection.
- The workflow is notebook-first by design, so reproducibility is centered on the notebooks and committed artifact bundles rather than on a packaged command-line application.

## Citation

If you use this repository, please cite the associated paper:

> Bowen Chen, Michael Winter, Torben Miny, and Tobias Kleinert,  
> *Optimal Executable Process Plan Generation for Capability-Constrained Modular Plants*.

See [CITATION.cff](CITATION.cff) for repository citation metadata.

## License

This repository is released under the [MIT License](LICENSE).
