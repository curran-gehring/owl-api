# owl-api review rules

Sent to the adversarial review lenses when a diff touches a matching path. Each rule records a defect that shipped or was caught once. Add a rule when a reviewer could have seen the defect in the diff.

## Units and Owl plan building
paths: src/owl_api/*

- Dollars are k$ at the API boundary and raw dollars inside Owl. Every dollar-valued field built in `_extract_schedule` (`solver.py`), including a newly added `YearlySchedule` field, divides the `plan.*` value by `K = 1000.0`. A field taken straight from the plan arrays comes out 1000 times too large next to the k$ inputs and the state-tax adjustment.
- `setHSA` is called only when an HSA balance is positive, and a positive balance with no matching HDHP contribution data makes the plan infeasible. Keep `hsa_savings_balances` at `[0, 0]` unless the change also supplies the year-indexed contribution data.
- Do not rely on `max_roth_conversion` as a hard cap. In testing, Owl 2026.4.8 returned a $100k conversion under a $50k cap. A change that depends on the cap, or raises the `100.0` default in `SolverOptions`, checks the returned `roth_conversion` values against it before reporting them.
- Owl's `Plan` takes `dobs=`, not `yobs=`. Check keyword names against the installed `owlplanner` (`>=6.0`) rather than its older documentation.
