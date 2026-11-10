# PNP-RW1 IT1: public evidence release

**Internally verified under team custody (PNP-RW1 IT1). Not externally certified.**

- `PNP-RW1_IT1_Research_Report.md`: the research report (confirmatory result, limitations, exploratory analyses).
- `PNPRW1_IT1_CONF.json`: the confirmatory runner record, unchanged (the result file named in the governed log).
- `PNPRW1_IT1_CONF_analysis.json`: the frozen analysis output, unchanged.
- `PNPRW1_IT1_posthoc_exploratory.tar.gz`: post-hoc EXPLORATORY scripts and outputs (not confirmatory). Its README
  marks the original E1 output as superseded by E1r and kept for traceability only.
- `confirmatory/results/`: byte-identical COPIES of the two JSON files above, at the path the exploratory scripts
  read. They are duplicates for convenience, not regenerated data; `EVIDENCE_SHA256SUMS.txt` shows the identical hashes.
- `EVIDENCE_SHA256SUMS.txt`: SHA-256 of every file in this folder.

The original seed commitment is at the repository root (`PNP-RW1_IT1_seed_commitment.txt`) and in Issue #1.

## Reproduce the exploratory analyses

From this folder (Python 3, standard library only):

```
sha256sum -c EVIDENCE_SHA256SUMS.txt
tar -xzf PNPRW1_IT1_posthoc_exploratory.tar.gz
for e in E1_step_price_decomposition E2_moves_survival_km E3_population_sensitivity E1r_E2r_review_corrections; do
  python3 -I -B posthoc_exploratory/$e.py | cmp - posthoc_exploratory/$e.txt && echo "$e reproduces"
done
```

This reproduces the exploratory analyses only. Reconstructing the confirmatory run itself also needs the frozen
source package, the seed, the beacon and the pinned environment, which are not part of this release.
