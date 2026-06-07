# graphed-numpy

A trivial numpy backend for `graphed`, proving the backend seam (M2). Operates on 1-D numpy bags: arithmetic, boolean filter, sum reduction, and opaque `map` callables (recorded as External nodes with a preservation-risk payload descriptor). Part of the [graphed-org](https://github.com/graphed-org) project.

```bash
pip install "graphed-core @ git+https://github.com/graphed-org/graphed-core-mvp@main"  # needs Rust
pip install "graphed @ git+https://github.com/graphed-org/graphed-mvp@main"
pip install -e ".[dev,docs]"
ruff check . && mypy && pytest tests/frozen/m2
```

Status: see `.graphed/state.json` and `CLAUDE.md`.
