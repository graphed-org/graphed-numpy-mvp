# M14 attempts — graphed-numpy (dask.array parity P3.8: apply_gufunc)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10 (freeze-M14-0)

- frozen suite authored; NON-VACUOUS (all tests fail: gn.apply_gufunc missing).
- one pre-freeze sanity fix (recorded): the projection test originally asserted that
  on_fail="pass" is conservative-everything, contradicting the FROZEN M5 policy (pass ->
  optimistic union; warn -> CONSERVATIVE; raise -> ProjectionError); re-pinned to the M5
  semantics, now covering all three modes through a gufunc node.

## Iteration 0/1 — IMPLEMENTING/REVIEW — 2026-06-10

- gufunc.py: parse_signature (one output, malformed refused), gufunc_form (core dims bound
  against operand forms; mismatch/unbound/rank/arity errors at record time), apply_gufunc
  (External "gufunc" node; PayloadDescriptor io_schema IS the signature; vectorize=False
  whole-array evaluation). Backend: op_form/gufunc + external_payload/gufunc; boundary_ops +=
  gufunc.
- frontend gap found and fixed in the graphed repo (its M14 iteration 1): record_external did
  not wrap backend op_form errors into provenance-located GraphedTypeError the way record_op
  does — gufunc binding errors surfaced raw.
- gates: frozen_tests 322/322 PASS · coverage 94% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green · sphinx -W clean.
