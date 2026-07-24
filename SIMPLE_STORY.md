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
from the first 8 numbers at 0.43 accuracy vs 0.03 to 0.10 for every other variant: the
front doll is where the task lives.

## 5. What a family can do that a loner cannot: disagree

Ask the inner doll and the full model to imagine the same future under the same
actions. On real states they agree; the training sentence made them. On garbage
embeddings nothing ever made them agree, and they do not.

That disagreement is an internal alarm, and it catches exactly the failure from
Symptom 3: it separates garbage goals from real ones at 0.92 to 1.00 detection on
every garbage family, including the cases where every simple trick fails (on one
model, embedding-norm and distance-to-center detectors fall to chance, 0.58, while the
disagreement signal still reads 0.96). Monoliths have no usable analog of this signal,
and even the random-groups variant only detects erratically (0.61 to 0.99). The
reliable alarm needs the ordered dolls.

## 6. Honesty box (all measured, all stated)

- The score itself still prefers garbage; the alarm detects it, but we have not yet
  wired the alarm into the planner. Detector demo, not yet a safer planner.
- Coordinate frames still do not transfer across retrains: a readout trained on run A
  fails raw on run B for every variant (we predicted partial success and were wrong;
  recorded). What recurs across retrains is the SUBSPACE: the best rotation inside the
  front block recovers 75% of the cross-seed discrepancy for our arms vs 40% for
  monoliths. Ordering consistency, not frame identity.
- The doll variants' full width reads a few points below monoliths on some runs
  (65 to 66.5 vs 70 to 71 on one seed; within noise averaged over seeds, reported
  anyway).
- One environment carries all planning claims (the maze benchmark is provably unable
  to measure planning, which we show); two planner families; two seeds; small models
  (ViT-tiny, 192 dims). Scale is untested.
- Encoder oversensitivity to tiny image shifts is unchanged by everything.

## 7. One breath

World models are monoliths: one unaccountable opinion, internals arranged by coin
flip, no smaller self inside, no ability to doubt. One line of training turns the
monolith into nested dolls sharing its numbers. The dolls are real: the small one runs
the whole show at an eighth the size, on two planners and two seeds, and the gap
decomposes into its two causes. And dolls can do what a loner cannot: when the inner
doll and the whole disagree, the model has caught its own nonsense, including the fake
goals its own score prefers.
