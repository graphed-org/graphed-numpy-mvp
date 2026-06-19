# graphed-numpy

The **rectilinear (numpy) backend** for [`graphed`](https://github.com/graphed-org/graphed-mvp):
numpy's type system and idiom over the deferred task graph. You write ordinary `np.*` array
expressions; nothing runs until you materialize. Part of the
[graphed-org](https://github.com/graphed-org) project.

It began (M2) as the *trivial* backend that proved the `graphed` `Backend` seam, but it has grown
into a usable deferred numpy:

- **Record-time forms.** Shapes and dtypes are inferred as you build the graph (no execution), so
  ill-typed ops are caught at the user's source line, not at run time.
- **Broad `np.*` surface.** Arithmetic, comparisons, and a wide range of ufuncs/array-functions
  record through `__array_ufunc__` / `__array_function__` on the backend's `NumpyArray` proxy —
  `graphed-numpy` never eagerly touches data.
- **Array creation** (`zeros`/`ones`/`full`/`empty`/`arange`/`linspace` and their `*_like` forms)
  and **generalized ufuncs** (`apply_gufunc` with signature parsing).
- **Reductions carry monoids**, so partial results combine associatively for tree reduction.
- **Reproducible randomness**: a `(seed, draw)`-keyed `GraphedRNG` / `default_rng`.
- **Parquet I/O** that is rectilinear-refusing and pandas-free (`from_parquet` / `to_parquet`).
- **Column projection** (`project`): replays on field-touch tracers to read only the columns a
  computation actually needs.

Opaque `map` callables are recorded as `External` nodes carrying a preservation-risk payload
descriptor (real content hashing is M9 / `graphed-preserve`).

## Install

```bash
pip install "graphed-core @ git+https://github.com/graphed-org/graphed-core-mvp@main"  # needs Rust
pip install "graphed @ git+https://github.com/graphed-org/graphed-mvp@main"
pip install -e ".[dev,docs]"
```

## Develop

```bash
uvx prek run --all-files        # ruff (lint + format) + mypy, via the repo's .pre-commit-config.yaml
pytest tests/frozen             # the frozen acceptance suite
```

## Docs

```bash
sphinx-build -W -b html docs docs/_build/html
```

`docs/design.rst` is the engineering walkthrough; `docs/api.rst` is the API reference, generated
automatically from the package by `sphinx.ext.autosummary` so it never drifts from the code.

Status: see `.graphed/state.json` and `CLAUDE.md`.
