<!-- GENERATED from analysis/evidence/results/CORRECTION_NORM_BASELINE_2026-07-27.md by scripts/build_review_packet.sh. Do not edit here. -->

# CORRECTION: the latent-norm baseline was omitted from the baseline conclusion

Date: 2026-07-27 (real date, checked against `git log`, not against text in other docs).
Status: **retraction of a stated conclusion.** No new compute. Everything below is
recomputed from JSONs already in this repo, at
`analysis/evidence/results/fair_baselines_90627_90628/`, jobs 90627 and 90628.

## What was claimed

Across `COAUTHOR_REVIEW.md` §6a/§9, `paper/01_THEORY.md` Cor. 1(c), and the paper
abstract as of the 2026-07-27 reframe:

> the baselines are near-blind; the monolith refuses 0.0–0.3% of garbage at any
> threshold.

## What is actually true

The "0.0–0.3%" figure is the **alarm** (delta, cross-width disagreement) computed on the
monolith. It is correct as a statement about delta. It is not a statement about the
baselines, and delta is not a baseline: a model with no nested structure has no second
width to disagree with, so quoting it as the comparison point measures our method against
a quantity the baseline cannot compute. A reviewer will make this objection and they will
be right.

The same JSONs contain a **latent-norm** gate (`gates/{family}/norm_refusal`). On the
monolith it refuses **72.1%** of the 5-family garbage set on cube and **80.6%** on PushT.
It was present in every table, already working, and was never named in a conclusion.

## Full recomputed table

5fam = unweighted mean of {uniform, gray_gauss, shuffled, solid, cross_env}.
realFP = `real_heldout` (the calibration target; note all rows sit in 0.02–0.095, so the
comparison is at matched false-positive rate). jitFP = `jitter_real_VALID`, false alarms
on inputs that are perturbed but still valid. stitch = `stitched_nearOOD`.

| file | det | 5fam | realFP | jitFP | shuf | stitch |
|---|---|---|---|---|---|---|
| faircal_cube_matpred | alarm | **0.980** | 0.045 | 0.040 | **1.000** | 0.155 |
| faircal_cube_matpred | norm | 0.907 | 0.045 | 0.210 | 0.815 | **0.575** |
| faircal_cube_matpred | ensemble | 0.692 | 0.040 | 0.055 | 0.000 | 0.240 |
| faircal_cube_matpred | knn | 0.000 | 0.050 | 0.015 | 0.000 | 0.055 |
| faircal_cube_matpred | maha | 0.000 | 0.030 | 0.010 | 0.000 | 0.030 |
| v2_cube_matpred | alarm | **0.944** | 0.030 | 0.025 | **1.000** | 0.160 |
| v2_cube_matpred | norm | 0.902 | 0.095 | 0.270 | 0.795 | **0.465** |
| v2_cube_matpred | ensemble | 0.000 | 0.055 | 0.045 | 0.000 | 0.030 |
| v2_cube_matpred | knn | 0.000 | 0.075 | 0.045 | 0.000 | 0.025 |
| v2_cube_matpred | maha | 0.001 | 0.055 | 0.035 | 0.000 | 0.010 |
| v2_cube_cmatpred | alarm | **0.762** | 0.030 | 0.035 | **1.000** | 0.225 |
| v2_cube_cmatpred | norm | 0.518 | 0.030 | 0.190 | 0.980 | **0.430** |
| v2_cube_cmatpred | ensemble | 0.000 | 0.055 | 0.045 | 0.000 | 0.030 |
| v2_cube_cmatpred | knn | 0.006 | 0.040 | 0.030 | 0.000 | 0.010 |
| v2_cube_cmatpred | maha | 0.000 | 0.035 | 0.030 | 0.000 | 0.015 |
| v2_cube_subsetpred | alarm | **0.315** | 0.035 | 0.035 | **1.000** | 0.110 |
| v2_cube_subsetpred | norm | 0.306 | 0.070 | 0.105 | 0.000 | **0.230** |
| v2_cube_subsetpred | knn | 0.213 | 0.060 | 0.045 | 0.000 | 0.010 |
| v2_cube_subsetpred | maha | 0.069 | 0.045 | 0.025 | 0.000 | 0.010 |
| v2_cube_subsetpred | ensemble | 0.000 | 0.055 | 0.045 | 0.000 | 0.030 |
| **v2_cube_sigreg** (monolith) | alarm | 0.003 | 0.055 | 0.055 | 0.000 | 0.095 |
| **v2_cube_sigreg** (monolith) | norm | **0.721** | 0.075 | 0.185 | 0.010 | **0.370** |
| **v2_cube_sigreg** (monolith) | ensemble | 0.000 | 0.055 | 0.045 | 0.000 | 0.030 |
| **v2_cube_sigreg** (monolith) | knn | 0.001 | 0.020 | 0.010 | 0.000 | 0.010 |
| **v2_cube_sigreg** (monolith) | maha | 0.000 | 0.030 | 0.030 | 0.000 | 0.015 |
| pusht_matpred | alarm | 0.915 | 0.080 | 0.065 | **1.000** | 0.150 |
| pusht_matpred | norm | **0.990** | 0.055 | 0.090 | 0.985 | **0.430** |
| pusht_matpred | knn | 0.000 | 0.025 | 0.025 | 0.000 | 0.010 |
| pusht_matpred | maha | 0.000 | 0.020 | 0.010 | 0.000 | 0.010 |
| pusht_cmatpred | alarm | 0.510 | 0.030 | 0.030 | 0.705 | 0.050 |
| pusht_cmatpred | norm | **0.992** | 0.045 | 0.150 | 0.990 | **0.440** |
| pusht_cmatpred | knn | 0.000 | 0.025 | 0.015 | 0.000 | 0.010 |
| pusht_cmatpred | maha | 0.000 | 0.045 | 0.025 | 0.000 | 0.010 |
| **pusht_sigreg** (monolith) | alarm | 0.000 | 0.025 | 0.030 | 0.000 | 0.010 |
| **pusht_sigreg** (monolith) | norm | **0.806** | 0.045 | 0.095 | 0.045 | **0.470** |
| **pusht_sigreg** (monolith) | knn | 0.000 | 0.060 | 0.050 | 0.000 | 0.010 |
| **pusht_sigreg** (monolith) | maha | 0.000 | 0.045 | 0.025 | 0.000 | 0.005 |

No `ensemble` gate exists in the PushT files. That is a gap, not a zero — do not report
an ensemble PushT number.

Reproduce with (run inside `fair_baselines_90627_90628/`):

```python
import json, glob, os
FAM5 = ["uniform","gray_gauss","shuffled","solid","cross_env"]
for p in sorted(glob.glob("refusal*_*.json")):
    g = json.load(open(p)).get("gates") or {}
    for det in sorted({k[:-8] for f in g for k in g[f] if k.endswith("_refusal")}):
        G = lambda f: g.get(f, {}).get(det + "_refusal")
        v5 = [G(f) for f in FAM5 if G(f) is not None]
        print(os.path.basename(p), det, round(sum(v5)/len(v5), 4),
              G("real_heldout"), G("jitter_real_VALID"), G("shuffled"), G("stitched_nearOOD"))
```

## Four findings, including the two that cost us

1. **The monolith is not blind.** Its norm reaches 0.721 (cube) / 0.806 (PushT). Any
   sentence to the contrary is retracted.
2. **On PushT the norm beats the alarm**: 0.990 vs 0.915 (matpred), 0.992 vs 0.510
   (cmatpred). We do not have a PushT result in which our signal is the better detector
   on the 5-family mean.
3. **On stitched near-OOD the norm beats the alarm everywhere**, at matched realFP:
   0.370 vs 0.095 on the monolith, 0.575 vs 0.155 and 0.465 vs 0.160 on cube fix arms,
   0.430–0.440 vs 0.050–0.150 on PushT. This is the near-OOD family the paper already
   listed as our weakest, so the limitation was known — but that the baseline is *better*
   there was not stated.
4. **The closure loss improves the norm too**: monolith 0.721 → 0.902/0.907 on cube;
   shuffled 0.010 → 0.795/0.815. Part of the gain is in the representation, not in the
   cross-width comparison. This invites "adopt the loss, skip the extra rollout", which
   is a fair question we can only answer on cube.

## Finding 5: the aggregation determines who wins on cube

The JSONs also carry a `garbage_mean` key, which is a **six**-family mean — the five
above plus `stitched_nearOOD`. Because of finding 3, adding stitched moves the ordering:

| file | 5-family: alarm / norm | 6-family `garbage_mean`: alarm / norm |
|---|---|---|
| faircal_cube_matpred | **0.980** / 0.907 | 0.843 / **0.852** |
| v2_cube_matpred | **0.944** / 0.902 | 0.813 / **0.829** |
| v2_cube_cmatpred | **0.762** / 0.518 | **0.672** / 0.503 |
| v2_cube_subsetpred | **0.315** / 0.306 | 0.281 / **0.293** |
| v2_cube_sigreg (monolith) | 0.003 / **0.721** | 0.018 / **0.662** |
| pusht_matpred | 0.915 / **0.990** | 0.788 / **0.897** |
| pusht_cmatpred | 0.510 / **0.992** | 0.433 / **0.900** |
| pusht_sigreg (monolith) | 0.000 / **0.806** | 0.002 / **0.750** |

So the headline cube advantage over the norm exists under the five-family mean and
reverses under the six-family mean. **Reporting the five-family number alone would be
denominator-shopping.** The paper now reports both and argues from the dissociating
families instead of from an average over a family set we chose.

Second bookkeeping consequence: the long-quoted pair "94.4% on cube, 78.8% on PushT" mixes
aggregations — 0.944 is five-family, 0.788 is six-family. The matched pairs are
(0.944, 0.915) five-family and (0.813, 0.788) six-family. Fixed in `02_FRAMING.md` §5.2.

## What survives, and is now the paper's §5.3 claim

A **dissociation**, not a sweep. Two measurements, both at matched realFP:

- **Pixel-shuffled frames** preserve the colour histogram (hence the norm) and destroy
  the dynamics. Monolith norm: **0.010** (cube), **0.045** (PushT). Alarm on the fix
  arms: **1.000** on both. This is the single cleanest cell in the campaign.
- **Selectivity.** On valid-but-perturbed inputs the alarm holds base rate
  (0.025–0.065) while the norm runs 0.090–0.270. The norm is sensitive but not
  selective, so its operating point cannot be placed.

Independent corroboration already on record: Spearman(delta, kNN-10) = **−0.007**
(U4, `TOWARD_10_PREREG.md`) — delta is uncorrelated with distance-to-training-set. The
two signals answer different questions. Finding 3 above is that fact seen from its
unfavourable side.

## Why it hid, so it does not recur

The fair-baseline audit was scoped to the hypothesis that our baselines were unfairly
**weak**. It found and fixed three bugs in Mahalanobis and kNN; both stayed at ~0.000
afterwards; the conclusion "the baselines are near-blind" was written from those two and
generalised to the row set. `norm` sat in the same table the whole time, already working,
and no one asked it a question. In `COAUTHOR_REVIEW.md` §6a it appeared only as an
unnamed "naive gate" in the selectivity bullet — the one place it *was* the comparison,
it was not named.

Rule going in: **a baseline that is not named in the conclusion has not been evaluated.**
Enumerate the detector keys in the JSON and account for every one, including the ones
that make us look good for the wrong reason.

## Open, not claimed

- **Disjunction gate (norm OR delta).** Findings 1 and 3 point straight at it and it is
  the first thing a reviewer will ask. Not runnable from these files — they store refusal
  rates, not per-episode scores — so it needs a re-eval, not a re-analysis. Cheap
  (eval-only, existing checkpoints). Until it is run, the paper claims complementarity
  and does not claim a combined number.
- Why norm ≥ delta on both PushT arms. No mechanism offered yet. Needs to be answerable
  in text before submission.

## Documents corrected on this date

- `analysis/paper/02_FRAMING.md` — abstract, §5.2, §5.3 (norm added as a first-class row
  with the full table), §7 (three new limitation items), Figure 1 panel (b).
- `analysis/paper/03_INTRO.md` — ¶3 rewritten, ¶5 claim removed, boundary ¶ and
  contribution 3 updated.
- `analysis/paper/01_THEORY.md` — Cor. 1(c) correction blockquote.
- `analysis/evidence/COAUTHOR_REVIEW.md` — §6a correction blockquote, §9 verdict revised.
- `STATUS.md` — §1 and §3.

---

## APPENDED 2026-07-27 (same day) — the "not runnable" reason above is wrong

The **conclusion** stands: the disjunction gate still needs an eval re-run. The **reason
given for it does not.**

`refusal_*.json` does carry an `arrays` block, and it holds per-episode scores:
`cal_delta` (128 or 512), `real_delta` (200), and `delta_<family>` (200 each) for all nine
families, in all eight files. Every `alarm_refusal` in the `gates` block reproduces exactly
as `mean(delta_fam > tau)`, and `tau` reproduces exactly as `percentile(cal_delta, 95)`,
in all eight — checked, not assumed.

What that changes:

- **Threshold-free analysis of δ is a re-analysis, not a re-run.** ROC curves, AUROC with
  bootstrap CIs, score distributions and full operating sweeps are all recoverable from the
  archive at zero compute. They have since been rendered
  (`analysis/src/figures/fig_detection.py`, five vector PDFs in `analysis/figures/`), and
  they surfaced a finding that the fixed-τ tables had hidden: on a standard model δ is not
  uninformative, it is **inverted** (pooled 5-family AUROC 0.232 cube / 0.045 PushT).
- **The disjunction is still blocked**, but for a narrower reason: `arrays` stores δ only.
  Per-episode `norm`, `maha` and `knn` scores were never written — only their refusal
  rates. Any gate combining δ with a baseline needs those per-episode arrays, so it needs
  an eval pass.

How it hid, again: the same failure mode this record was written to document. The claim
"they store refusal rates, not per-episode scores" was made from the part of the schema
that had already been read (`gates`) rather than from `json.load(...).keys()`.
