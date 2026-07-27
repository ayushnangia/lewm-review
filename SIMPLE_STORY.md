<!-- GENERATED from analysis/evidence/THE_SIMPLE_STORY.md by scripts/build_review_packet.sh. Do not edit here. -->

# The simple story, v3 (canonical): from monolith to Russian dolls

One narrative, final numbers, everything measured. Supersedes v1/v2; the experiment
receipts live in `PROGRESS_SUMMARY.md` and `results/`.

## 1. The machine

A robot compresses each camera image into a list of 192 numbers. A predictor imagines
how the list changes under actions. A planner tries hundreds of action plans and picks
the one whose imagined final list is closest to the goal image's list, subtracting
number by number, every slot counted equally. This is the LeJEPA/SIGReg recipe
(Balestriero & LeCun), which also forces the cloud of lists to be round: equal spread
in every direction.

## 2. The problem: the model is a monolith

One machine, one opinion about every image, one size. We measured three symptoms, and
they are one disease.

**Symptom 1: its numbers are filed by luck.** Train the identical model twice, changing
only the random start. The runs learn the same things: a fitted translation carries
96.5% of one run's information into the other. But the filing is unrelated: agreement
between same-numbered slots is 0.003, where 0 is coin flip. The math says why: nothing
in either training loss prefers any arrangement, and measurement confirms nothing else
picks one either.

**Symptom 2: no smaller model exists inside it.** Keep only the first 24 of 192 numbers
and the SCORE still works: 70.5% success vs 70.0% with everything, because the content
is smeared so redundantly that any two dozen numbers can serve the comparison. But make
the IMAGINATION run on those 24 numbers and the model collapses to 54% (guessing scores
48). You can read a piece of a monolith. You cannot run a piece of a monolith: no
self-contained sub-model exists at any size.

**Symptom 3: it cannot doubt itself.** Show it garbage as a goal image (TV static, gray
mush, scrambled frames, photos of a different task) and its score says the garbage is
EASIER to reach than a real goal, 0.67 to 0.98 times the cost, on every model and every
garbage type we tested. One opinion means nobody inside to object.

## 3. The change: one sentence of training

At every training step, draw a random cutoff d. The first d numbers, by themselves,
must predict the first d numbers of the future. Six lines of code, no new networks, no
labels; training is bit-identical to stock when the flag is off.

This changes what kind of object the model is. The monolith becomes a family of nested
models sharing one set of numbers: a complete 16-number model inside a 24-number model
inside the full 192, like Russian dolls. The physics of the task decides what earns a
front seat: content that stays predictable longest.

## 4. Proof the dolls are real (and how much each ingredient buys)

**The small doll runs the whole show.** Keeping only the first 16 numbers, in score and
imagination, plans at 74.0% vs the model's own 73.5% full width, statistically
indistinguishable. Every monolith variant collapses to 49 to 57%. Replicated on a
second training seed (69.5/70.5/69.0 at d=8/16/24 vs 66.5 full) and, crucially, on a
second planner family: a gradient-based planner shows the identical pattern (monoliths
50/51/50, dolls 71.5/72.0). The gap survives a 10x planner search budget, so it is not
a solver artifact.

**Decomposed, with its own control.** We trained a variant with the same closure
pressure on random slot groups instead of nested prefixes. It does not collapse
(mask-robustness alone is worth +10 to +13 points over monoliths, p below 5.4e-4), and
the ordered version beats it by a further +6 to +10.5 points (paired, p = .036 to
.0001). So the headline gap splits cleanly: having any runnable inner model, plus
ordering the inner models, each separately significant.

**Nothing is hollowed out, and the task moves to the front.** The numbers past the
cutoff still carry full information about the present (suffix readout 0.989, equal to
full width), so this is genuine nesting, not shrinkage. And the block's position reads
from the first 16 numbers at 0.43 accuracy for the ordered dolls vs 0.10 for the
monolith, with the unordered control in between at 0.33: the front doll is where the
task lives, and ordering it is what puts the task there.

## 5. What a family can do that a loner cannot: disagree

Ask the inner doll and the full model to imagine the same future under the same
actions. On real states they agree; the training sentence made them. On garbage
embeddings nothing ever made them agree, and they do not.

That disagreement is an internal alarm, and it catches exactly the failure from
Symptom 3. Calibrated on nothing but real goals, one threshold refuses 94.4% of
garbage while wrongly refusing only 3.0% of real held-out goals (audited; 78.8% at
8.0% on the second environment). Fairly implemented standard detectors
(Mahalanobis, kNN, even a two-model ensemble) calibrate correctly and then miss the
hard families almost entirely (0% on scrambled pixels where the alarm scores 100%).
Monoliths have no usable analog of this signal, and even the random-groups variant
only detects erratically.

> **Correction, 2026-07-27.** That detector list leaves out the one that works. The
> **length of the embedding vector** — the simplest gate there is, available on a
> monolith with no changes at all — refuses 72% of garbage on cube and 81% on the second
> environment, and on the second environment it beats the alarm outright. So "monoliths
> have no usable analog" is false as written: they have no analog of *this* signal, which
> is not the same claim.
>
> What is still true, and is the real point: the norm asks "does this input look
> unusual?" and the alarm asks "do my own dynamics still hold here?" — and those come
> apart. Scramble a frame's pixels and its colours are untouched, so the norm sees
> nothing (1–4.5%) while the alarm sees everything (100%). Jiggle a real frame and the
> norm cries wolf (9.5–27%) while the alarm stays quiet (2.5–6.5%). Going the other way,
> on frames stitched from two real halves the norm does better than the alarm. Two
> different questions, two different blind spots.
>
> Full recomputed table and how it slipped through:
> `results/CORRECTION_NORM_BASELINE_2026-07-27.md`. The reliable alarm needs the ordered dolls. The same
signal, read during planning, predicts which plans will fail before acting (AUROC
0.74). And composed end to end on a mixed goal stream, the gated planner keeps real
throughput within noise while cutting garbage executions from 200 to 11.

## 6. Honesty box (all measured, all stated)

- The score itself still prefers garbage; the alarm catches it. The closed-loop
  numbers above are component-composed from archived measurements (independence
  stated), not a live gated rollout.
- Training small directly also plans well (a native 24-dim monolith hits 70.5, equal
  to the big one). The dolls' value is one checkpoint serving every width plus the
  alarm and confidence signals, which no monolith of any size supports.
- The alarm's honest boundary: physically impossible collages of real frames mostly
  slip through (16 to 22% caught). Far-off-manifold garbage is where it is sharp.
- Coordinate frames still do not transfer across retrains: a readout trained on run A
  fails raw on run B for every variant (we predicted partial success and were wrong;
  recorded). What recurs across retrains is the SUBSPACE: the best rotation inside the
  front block recovers 75% of the cross-seed discrepancy for our arms vs 40% for
  monoliths. Ordering consistency, not frame identity.
- The doll variants' full width reads a few points below monoliths on some runs
  (65 to 66.5 vs 70 to 71 on one seed; within noise averaged over seeds, reported
  anyway).
- Two environments carry the planning claims (cube: three seeds; PushT: two seeds,
  where the monolith falls from 92.5 to 4.5 and the fix holds 91.0 to 85.5); the maze
  benchmark is provably unable to measure planning, which we show. Two planner
  families. Small models (ViT-tiny, 192 dims); scale is untested. One uniform-arm
  PushT retrain diverged (disclosed).
- Encoder oversensitivity to tiny image shifts is unchanged by everything.

## 7. One breath

World models are monoliths: one unaccountable opinion, internals arranged by coin
flip, no smaller self inside, no ability to doubt. One line of training turns the
monolith into nested dolls sharing its numbers. The dolls are real: the small one runs
the whole show at an eighth the size, on two environments, two planners and up to
three seeds, and the gap decomposes into its two causes. And dolls can do what a loner cannot: when the inner
doll and the whole disagree, the model has caught its own nonsense, including the fake
goals its own score prefers.
