# NQ slow-state diagnostic interpretation

Date: 2026-08-31

Scope: research-only consumption of `foundry.nq_mnq_slow_state_stratification.v1` artifacts from PR #7. No runtime, routing, trading, or promotion authority.

## Immutable evidence consumed

- H12 / 1.0vol: artifact `9755025237`, `sha256:9c4bc2689a2cd3b002a9c3af5ba3b04efb1d09fcaee4c10e5d22b8ea50ad399f`
- H24 / 0.5vol: artifact `9754975217`, `sha256:98ebe823f4f423b9d967e7e4f79de07e828245071cbea54441b14d1ef190139d`
- Both come from workflow run `33383805912` at source head `f197b2e8db6a4ec50e36d760f4a9082cd0d7d814`.

The slow state is causal by construction: only the previous completed NQ futures trade week is attached, and the state never enters prediction fitting.

## What the result says

The strongest cross-horizon directional signature is on the predeclared volatility state, not trend or drawdown.

For `nq_expanding - mnq_expanding`, the artifact stratification reports:

| Prior NQ vol state | Weeks | H12 mean delta | H12 win fraction | H24 mean delta | H24 win fraction |
| --- | ---: | ---: | ---: | ---: | ---: |
| high | 12 | +6.08 pts | 0.58 | +12.95 pts | 0.75 |
| low | 29 | -0.83 pts | 0.34 | -4.93 pts | 0.41 |
| normal | 48 | +3.37 pts | 0.60 | -6.01 pts | 0.52 |

The high-vs-low sign is consistent across both horizons. The pooled-minus-MNQ comparison is materially weaker and does not show the same stable low-state sign.

## Critical confound

The apparent high-volatility regime is not independently distributed through this OOS sample. All 12 `high` weeks occur in 2025. That means the strongest apparent state signature is entangled with the same calendar transfer period the diagnostic was intended to explain.

Within the weekly rows, the NQ-minus-MNQ mean delta in high-vol 2025 is positive at both horizons, while low/normal 2025 is materially weaker. This is suggestive, but it is not enough to infer a reusable routing rule because there is no older high-vol OOS support in this experiment.

## Decision

**Do not promote a state gate or routing rule from #7.**

The result is useful because it narrows the next falsification target: test whether the NQ-vs-MNQ relative edge under causally-known high NQ volatility survives in an independent period containing high-volatility weeks outside 2025.

The next experiment should be predeclared before looking at its PnL and should preserve the existing model/prediction definitions. It should only test the interaction:

`prior completed NQ vol_state in {high, low, normal} x (nq_expanding - mnq_expanding)`

Acceptance should require the high-vs-low ordering to reproduce across both H12/1.0vol and H24/0.5vol in an independent time block. If independent high-vol support cannot be obtained, this lane should stop at diagnostic evidence rather than manufacture a routing policy from a 12-week, 2025-only slice.
