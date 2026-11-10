# PNP-RW1 IT1 Research Report: Memory-Informed Navigation under Full Work Accounting

6 November 2026 · Tim Nordin (custodian); BCOT research team

**Internally verified under team custody (PNP-RW1 IT1). Not externally certified.**

**Status:** Revision 3, prepared for public release on 6 Nov 2026. After team review, revision 2 corrected the E1 denominator, the E2 interpretation and the mesh wording. Revision 3 adds the details of the evidence release. No correction touches the confirmatory result.

## Summary

The confirmatory test returned **DETERIORATION**. The memory-informed walker (W1) needed a geometric mean of **1.358 times** the accounted work of a memoryless random walker (W0). The 95% CI was 1.310 to 1.408, entirely above the pre-registered 1.25 line.

The custodian reconstructed the run on a separate machine, from the committed seed and an independently fetched beacon. It gave **MATCH: 196,762 values compared, 0 mismatches**.

The finding is narrow. It shows that one visited-set navigation rule, under one declared cost model and budget, lost on five fixed graph families. It says nothing general about memory, about BCOT's Witness, or about P versus NP.

Exploratory analyses after the result suggest that W1's higher price per move explains much of the loss. They are diagnostics, not an isolated causal test, and they do not establish that W1 navigated better or worse.

The main lesson is about design. Before freezing, we never calculated how much better W1 had to navigate to pay for its bookkeeping. On this population and cost model, W1 needed roughly 27 to 59% fewer moves than W0, depending on the family, just to break even. On mesh it showed no observed reduction at all. We now require a pre-freeze discrimination gate (PFD-1) for future confirmatory work.

## Question and hypotheses

PNP-RW1 asked whether retained search state earns its keep once every operation it needs is charged. This study tested only **H2**, on the RGB-221C0 graph generators.

| Hypothesis | Question | Status in this study |
| --- | --- | --- |
| H1 | Does graph geometry predict W0's work? | **NOT\_EVALUATED**. The geometry instrument did not qualify on these graphs. This was recorded before any seed existed. |
| H2 | Does a memory-informed policy (W1) reduce accounted work relative to W0? | **Tested: DETERIORATION** |
| H3 | Does geometry predict W1's benefit? | **NOT\_EVALUATED**, for the same reason as H1 |

NOT\_EVALUATED is not a negative result. H1 and H3 were never tested.

**Scope recorded before the run.** RGB-221C0's mandatory waypoints are dominators computed offline, not obligations imposed on the walker. So the study tests visited-state memory in plain directed search. It does not test retained obligations, lawful-state history, or Witness as BCOT defines it.

## Design

The study compared two random walkers from a start vertex s to a target t, on fresh graphs drawn from a newly committed seed. The two walkers ran under the same work budget.

**Population.** There were 40 fresh instances in each of five reachable families: funnel, bypass, mesh, trap and trap\_hostile, for 200 in total. A further 40 blocked instances were used only to check that the exact baseline certifies unreachability. The 144 historical development cases were never used confirmatorily.

**Policies.**

- **W0 (memoryless).** At each vertex it reads the out-list length and moves to one uniformly chosen out-neighbour. At a sink it restarts at s.
- **W1 (memory-informed).** It reads every out-neighbour and looks each one up in a visited set. It moves uniformly to an unvisited neighbour if there is one, otherwise to any neighbour. The visited set is kept across restarts.
- **X (exact baseline).** A breadth-first search that certifies reachability and sets the budget.

**Cost model (Amendment A1).** Every operation actually performed costs one unit.

| Step | Charged operations | Units |
| --- | --- | --- |
| W0, non-sink vertex | goal test, degree read, one indexed edge read, move | 4 |
| W1, non-sink vertex of out-degree d | goal test, degree read, d edge reads, d visited-set lookups, one insertion if new, move | 2d + 3 or 2d + 4 |
| Sink, either walker | goal test, degree read, restart | 4 |

A1 corrected an earlier draft that charged W0 for reading every out-edge. That draft overcharged W0, which favoured W1. The correction was right, but its effect on W1's break-even was never calculated (see What we would have done differently).

**Budget.** Each run stops when its work exceeds B = k × W\_X, where W\_X is the exact baseline's work on that instance. k = 2 was fixed by a rule that used only W0 on the development cases: the smallest k at which W0's median per-instance success probability reached 0.5. W1 was never run on any RGB case before the confirmatory run. Each instance got 32 runs per walker, so there were 6,400 runs per walker.

**Primary statistic.** For each instance i, RMST is the mean of capped run costs. The effect is the geometric mean of the per-instance ratios:

GM = exp(mean over the 200 instances of ln(RMST of W1 on instance i ÷ RMST of W0 on instance i)). A GM above 1 means W1 used more accounted work.

The 95% CI comes from 10,000 bootstrap resamples of the 200 instances, using a keyed random stream and nearest-rank percentiles.

**Verdict rules, fixed before the run.**

- IMPROVEMENT if the CI's upper bound is below 0.8.
- DETERIORATION if the CI's lower bound is above 1.25.
- Otherwise INCONCLUSIVE.
- Any implementation defect makes the result DEFECT\_INVALID.
- A walker whose success rate trails its comparator's by more than 5 percentage points cannot win a work comparison (the cheap-failure rule).

## Custody and execution

The experiment ran once, from a single seed committed before its randomness could be known. A team custodian then reproduced the result on his own machine.

**Roles.** The executing agent ran the experiment in a cloud workspace. The custodian, Tim Nordin, worked on his own WSL2 machine, which the executing workspace could not reach. The custodian is a team member and is not independent of the team.

| Time (UTC) | Event |
| --- | --- |
| 2 Nov 2026 | IT1 adopted. The custodian qualified his environment and passed a rehearsal: the genuine test set gave MATCH, and a doctored set gave MISMATCH with 17 discrepancies. |
| 2 Nov, 23:29:25 | Freeze acknowledged. The custodian's checker verified the frozen identities and all 72 manifest lines against his own copy of the package. |
| 2 Nov, 23:31:24 | One seed generated and committed by hash. The beacon round, 33706040, follows by rule from the commit time plus 72 hours. |
| 3 Nov, 00:07:43 | Commitment published as GitHub Issue #1 in the public repository bcot-research-commitments. The time is GitHub's server record, about 71 hours before the beacon round. |
| 3 Nov, 00:15:52 | Custodian's acknowledgement of the commitment recorded (SEED-RECEIPT). |
| 5 Nov, 23:31:24 | drand quicknet round 33706040 published. |
| 6 Nov, 04:02:46 | Custodian fetched the round himself and verified its signature with the frozen verifier. |
| 6 Nov, 04:16:10 to 04:16:16 | The only confirmatory attempt. It completed with zero defects. |
| 6 Nov | Custodian reconstruction: MATCH, 196,762 comparisons, 0 mismatches, 0 missing, 0 uncompared. |

**What the reconstruction checked.** It was written separately from the runner, working from the specification. It regenerated all 240 instances and 12,800 runs from the committed seed and the custodian's own beacon. It then compared every recorded value and every statistic. It also confirmed that its own script, the runner, the seed and the result file matched the frozen and committed identities.

## Confirmed result

**H2: DETERIORATION.** GM = 1.35831356, 95% CI 1.31018534 to 1.40823769, n = 200 instances. The lower bound clears the 1.25 threshold.

| Measure | W0 | W1 |
| --- | --- | --- |
| Successful runs (of 6,400) | 3,945 (61.6%) | 3,481 (54.4%) |
| Instances where W1's mean capped cost was higher / lower / equal |  | 176 / 20 / 4 |

The exact baseline correctly certified all 40 blocked instances. No implementation defects were recorded.

**By family (descriptive only).** The protocol fixes one pooled verdict. Family figures are context, not five separate verdicts.

Mesh has the highest ratio, although both walkers succeeded in every mesh run. That is consistent with W1's higher price per move (see exploratory analysis E1), but it is not an isolated causal test.

| Family | GM, W1/W0 | 95% CI | W0 success | W1 success | Note |
| --- | --- | --- | --- | --- | --- |
| funnel | 1.0561 | 1.0253 to 1.0911 | 13.7% | 10.9% | Closed two-cycle traps: success measures avoiding entrapment within the budget, not escape |
| bypass | 1.1228 | 1.0811 to 1.1655 | 28.6% | 20.5% | Closed two-cycle traps, as funnel |
| mesh | 1.8005 | 1.7741 to 1.8277 | 100% | 100% | Both walkers took a median of 5 moves on successful runs |
| trap | 1.4580 | 1.3502 to 1.5845 | 83.6% | 72.1% |  |
| trap\_hostile | 1.4855 | 1.3880 to 1.5946 | 82.3% | 68.4% |  |

W1 had the higher cost ratio in every family, and lower success in four of the five.

## What the result claims, and what it does not

**The claim.** On the five RGB-221C0 families, with k = 2, the visited-set rule W1 needed more accounted work than uniform random W0 under cost model A1. The difference was large enough to clear the pre-registered DETERIORATION threshold.

**Not claimed:**

- **That memory makes search worse in general.** One rule for using memory was tested, at one price, on one population.
- **Anything about BCOT's Witness.** W1 remembers where it has been. It carries no provenance, no lawful-state history and no obligations, and nothing in these graphs required history to be kept.
- **Anything about P versus NP, physics or BCOT foundations.**
- **Results at other budgets.** Only k = 2 was tested. Whether W1 does better with more room is open.
- **Full physical cost.** The work is accounted under the declared operation-cost model. Memory allocation was not measured, and wall-clock times are diagnostics the operator vouches for, not reconstructed values.
- **That the population represents search problems at large.** The verdict pools five chosen families equally. Dropping mesh, as an exploratory check, gives about 1.27 with an approximate CI of 1.22 to 1.32, which no longer clears 1.25. The direction holds, but the strength of the verdict depends on the mix.

**Statistical scope.** The bootstrap resamples the 200 instances. The 32 runs per instance are repeated measurements of one instance, not independent graphs. The interval describes uncertainty about this population under these assumptions, not about arbitrary graphs.

## Exploratory analyses (after the result)

These analyses were run after the verdict was known. They read the frozen record without changing or rerunning anything, and they are not confirmatory. They suggest that **W1's higher price per move explains much of its deterioration**. They do not isolate that cause experimentally, and they do not establish that W1 navigated better.

### E1. Price per move and the break-even it implies

Across all runs, W1 made 33% fewer graph moves than W0 (556,260 against 835,039) but used 27% more accounted work (4.71 million units against 3.72 million). Its 1.30 million visited-set lookups and 1.30 million edge reads account for the difference.

| Family | Work per graph move, W0 / W1 (units) | Fewer moves W1 needs to break even | Median moves on successful runs, W0 / W1 |
| --- | --- | --- | --- |
| funnel | 4.03 / 5.52 | 27% | 11 / 12 |
| bypass | 4.19 / 10.22 | 59% | 4 / 1 |
| mesh | 4.20 / 7.49 | 44% | 5 / 5 |
| trap | 4.65 / 10.04 | 54% | 84.5 / 59 |
| trap\_hostile | 4.65 / 9.98 | 53% | 101 / 69.5 |

**Definitions.** Work per graph move is all accounted work, including goal tests and sink restarts, divided by the number of graph moves. That is why W0 sits above the 4 units of a single non-sink step. The break-even reduction is 1 − (W0's work per move ÷ W1's), assuming comparable trajectories.

**Correction to an earlier draft.** The first draft divided total work by goal tests and labelled the result "units per step" (3.5 to 4.0 for W0). That label was wrong. A re-priced estimate built on that denominator (pooled ratio about 1.00) is withdrawn from this report. Both remain in the preserved E1 output.

**Limits of E1.** The median moves cover successful runs only. W1 succeeded less often, so the two medians describe different subsets of runs (a selection effect). They cannot show that W1 navigated better.

### E2. Reaching the goal, move for move

E2 uses every run, to avoid that selection effect. It estimates the probability of reaching t within m moves (Kaplan–Meier, with runs that hit the budget treated as stopped at the moves they had made).

| Family | Within | W0 | W1 |
| --- | --- | --- | --- |
| trap | 100 / 200 moves | 46% / 65% | 51% / 71% |
| trap\_hostile | 100 / 200 moves | 41% / 57% | 44% / 61% |
| mesh | 5 moves | 65% | 64% |
| funnel | 50 moves | 13% | 11% |
| bypass | 25 moves | 29% | 22% |

The plain observed fractions of all runs that succeeded within 200 moves point the same way on the trap families: 64.8% for W0 against 69.6% for W1 on trap, and 57.4% against 60.3% on trap\_hostile. Over the whole budget, though, W1's success rate was lower in both families.

Run end states in funnel and bypass do not tell one story:

| Family | Ended in a closed trap, W0 / W1 | Still live when the budget ran out, W0 / W1 |
| --- | --- | --- |
| funnel | 1,105 / 1,133 | 0 / 7 |
| bypass | 889 / 643 | 23 / 368 |

**Interpretation.** The move-based comparisons suggest a possible W1 navigation advantage on the trap families over some move horizons. But budget exhaustion depends on the policy, the path and the costs met along it, and those may be related to reaching t. So these estimates cannot establish an intrinsic navigation advantage, or show what either walker would do with unlimited moves.

In funnel and bypass, W1 had lower success within the work budget. The end states differ between the two families: in bypass, W1 ended in a closed trap less often than W0 and far more often ran out of budget while still live. So W1's lower success cannot be put down uniformly to more frequent entrapment. How routing, trapping and early budget exhaustion each contributed remains unresolved. End states are not a measure of trap entry either, because the walkers get different numbers of moves for the same budget.

On mesh, W1 showed no observed reduction in moves while paying more per move, so its higher accounted cost followed from its observed behaviour. W0's move counts varied within 38 of the 40 mesh instances, and observed paths ranged from 2 to 8 moves. Mesh did not make a W1 gain impossible; it gave little observed room for one under this policy.

### E3. Sensitivity to the population

We recomputed the pooled ratio with each family left out in turn, using a plain bootstrap rather than the frozen keyed stream.

| Left out | GM | Approximate 95% CI |
| --- | --- | --- |
| none | 1.358 | 1.311 to 1.409 |
| funnel | 1.447 | 1.392 to 1.503 |
| bypass | 1.425 | 1.368 to 1.483 |
| mesh | 1.266 | 1.220 to 1.316 |
| trap | 1.335 | 1.283 to 1.389 |
| trap\_hostile | 1.328 | 1.276 to 1.383 |

The ratio stays above 1 whichever family is left out. Without mesh, the interval no longer clears the 1.25 line. Mesh, where W1 showed no observed reduction in moves, carries much of the verdict's strength.

**What a clean test would need:** a prospectively designed experiment that varies the information price and the navigation rule separately, on a fresh population.

## Verification limitations

Team custody genuinely separated execution from checking. It did not make the check independent of the team. Each limit below is recorded in the custody file.

| What was assured | What limits that assurance |
| --- | --- |
| The custodian checked the evidence on a machine the executing workspace could not reach | The custodian is a team member, not an outside reviewer. IT1 claims separation, not independence. |
| The custodian checked everything against hashes and tools he held | **Every custodian tool came from the executing side.** Setup, freeze check, rehearsal and the reconstruction wrapper were all operator-written. He ran them on his machine; he did not develop them. |
| A reconstruction written from the specification reproduced every value | It shares the original graph generators and the specification with the runner. Agreement shows reproducibility. A wrong shared assumption would reproduce just as well. |
| A public timestamp fixed when the commitment existed | It shows the commitment existed by 3 Nov, 00:07:43 UTC. It cannot show that no other seed was generated privately; exactly one seed per freeze is an obligation, backed by custody records. |
| The custodian acknowledged the commitment before the beacon round | His receipt file was not transferred. The operator rebuilt it from its byte-identical copy, matched to the SHA-256 the custodian reported. |
| The Issue's creation time was checked | The operator could not open GitHub Issue pages from the executing workspace. The time rests on the reviewer's check and the custodian's account. The published file itself was verified byte-identical by a clone. |
| Results were reviewed by several team members | The reviews were separate reviews by AI agents and a human working from shared material. They were not organisationally independent. |

**Bottom line:** the evidence would be hard to alter without detection. It is not externally certified, and IT1 never claims it is.

## What we would have done differently

**We would have checked, before freezing, whether the experiment could tell the hypotheses apart.** We did not, and that is the study's main design weakness.

Everything needed for that check was in hand before the seed existed:

- **The cost model.** A1 fixed W0 at 4 units per step and charged W1 2d + 3 or 2d + 4 units per step, rising with out-degree.
- **The graph structures.** The out-degree distribution of every family was known from the development cases.
- **W0's behaviour.** The W0-only calibration measured how many moves W0 needs per family.

From those three, a short calculation would have shown how much better W1 had to navigate to pay for its bookkeeping. That is roughly 27 to 59% fewer moves, depending on the family. It would also have flagged mesh, where W0 already reached t in 2 to 8 moves, as offering little room for W1 to show a gain. That calculation needed no W1 runs on RGB cases, so it would not have broken the rule against them.

We asked whether W1 beats W0. We should first have asked whether the design gave W1 a real chance to show an advantage, and whether a loss could be told apart from mere bookkeeping expense. As designed, a loss could not be told apart from the price of information, which the cost model largely fixed in advance. The primary result cannot separate bad navigation from expensive information.

**Why it was missed.** Most of the review effort went into whether the result could be trusted: the freeze, the custody chain, the reconstruction, and resistance to tampering. That work found real flaws and made the result reproducible. But much less effort went into whether the experiment was informative. We became very good at proving the machine gave the answer it was built to give, before being equally rigorous about whether we had asked it the right question.

The A1 correction shows the pattern. Lowering W0's charge removed a real bias, and was right. It also raised the reduction in moves W1 needed to break even, and nobody recalculated it.

## Methodological decisions adopted

The team has adopted the **Pre-Freeze Discrimination Gate (PFD-1)** for all future confirmatory experiments. It is a short technical review held before any freeze, not a new governance framework.

**PFD-1 requires, before a freeze:**

1. The structural hypothesis under test, stated exactly.
2. Break-even or feasibility conditions, calculated from development data.
3. A check of whether the expected outcome could follow from the cost model alone.
4. At least one competing mechanism that could produce the same result.
5. Measurements that would tell the competing mechanisms apart.
6. Two design reviews before implementation.

**Stopping rule.** An experiment that cannot tell the proposed mechanism apart from a simpler cost or implementation artifact does not qualify for a confirmatory freeze.

**Refinements adopted with PFD-1:**

- **Every family must have a real chance to discriminate.** Each family in the primary pooled verdict must be able, defensibly, to tell the hypotheses apart. Families that cannot may stay in a study as controls, but only if that role is declared before the freeze, so they cannot dominate the primary result. Mesh would likely have been flagged by this test: W0 already reached t in 2 to 8 moves, leaving little observed room for a W1 gain. That is a judgement from observed behaviour, not a proof that a gain was impossible.
- **Reviews are described as they were done.** Reviews by AI agents that share the same information and working environment are reported as separate reviews, not organisationally independent ones.

**What does not change.** For confirmatory runs, the freeze, single-seed commitment, custody and reconstruction requirements stay. Exploratory work should stay light and fast.

## Open questions and next steps

IT1 closes as conducted. Its verdict is not revised, rerun or replaced. The open questions split into an ordinary algorithmic one and the BCOT question proper.

**Question A: can retained information improve navigation enough to pay for itself?** This is a standard algorithms question. A new design would vary the information price and the decision rule separately: for example, a walker that samples one neighbour against a walker that scans all of them. It would also vary the budget, and include structures where memory can matter. It must pass PFD-1 and use a fresh population.

**Question B: when is relational history necessary to decide which transitions are lawful?** This is closer to BCOT. The test must not make history necessary simply by writing a rule that demands it. That would show only the consequence of our own rule. A stronger design compares systems whose current observable states are identical but whose histories differ. It then asks whether an independently justified correctness or admissibility condition forces the difference to be kept. It should keep two things apart:

- history that correct execution requires;
- optional history used to search more efficiently.

It should also keep countermodels in which memory gives no benefit. Making memory look useful through convenient restrictions would mislead as much as making it look expensive by construction.

**Unresolved by this study:** H1 and H3, which remain NOT\_EVALUATED; W1's performance at budgets other than k = 2; and any claim about Witness as BCOT defines it.

**Review before release:** the team reviewed this report twice, with particular attention to whether any conclusion goes beyond what the experiment showed. The review corrections are described in the exploratory section and in the archive README.

## Evidence record

All frozen files and custody records are preserved unchanged. The custody file is append-only. Every identity below is a full SHA-256.

| Item | SHA-256 |
| --- | --- |
| IT1 designation (adopted) | 118f04be552be506e83334bdb141970ac7b35ba1e872a3390e8aa4fdbfd773d0 |
| Binding amendment A2r4 | ed8f37795c5c1f7c02bd8e34ecd25a20d1ece6893f9930c4f42bbac438245cbb |
| Freeze manifest | ddf58bb8d972252f280a57727fb9ac90465208b1d8f34006e37f57175d6e1ba0 |
| Runner (FREEZE-ID) | 237bba3d6f227afeea00f6ee820d07837dcaf898b1ec7511d71d064ff5cb0f26 |
| Analysis (FREEZE-ID) | 0f892c66a759601d0e704a2a7e78ff43858fbd754d8b3434fd8a342cddc41d6f |
| Lineage (FREEZE-ID) | 4e31f1b09f374ec56698c24478894fc241eb519879b76ceb7cbcad16a463c308 |
| Beacon verifier (FREEZE-ID) | 8e089bc1000dc8a17f7c4c8328a8458592ef5ab16feb7a162cad3f3351ac0321 |
| Reconstruction (FREEZE-ID) | 45be7e1197a245947ece11d69e55bd351a813668a3d85ab056604f37480bded2 |
| Seed commitment (seed file) | e09efc1879936bddb3e988875c392fd51963401ae83f05f052201cd7cde037d4 |
| Published commitment record | 64c7db934a960b6437d4cdb6b9def7f1591ce9dd48c893bbc892c8427239bd2b |
| Custodian acknowledgement | b69e9eb8d29099ae2bba2db4aeb903df8d4ca0c75caa655fe514190f54b1a3f5 |
| Beacon, quicknet round 33706040 | 92e7dfe17add6cdab2e85d7ecf6616394cd6442f4527df8d587d5cd1254565bb |
| Runner record (result) | 6dea1a3fa86956ad4686186f4d08ce1844d97717e17f8456bdc9bfe4883b2e25 |
| Analysis output | 1bfb7b0a020e574287e3f7a86d7217eff99a5b14e6d73a2978fcb3c6d1980407 |
| Governed execution log (after POSTRUN) | ce5645e85c2c18363d1233de99aff89c4b9d53e8687e330cfcddc498dc64f093 |
| Result anchor | 87912f9d315c51aa7b296438a975d1efcbd5f8e9613d0970ad15a1ea353ddece |
| Custodian reconstruction report | c1457e40fb61ade9b4473644ce8ac96f3d70b9f3fd57fc0e9612374879d6fe0d |
| Exploratory E1 output (superseded by E1r) | c989e116fc4f30cdd5202729d8cabc82cf355d749d5c553c2fc22dc486192b45 |
| Exploratory E2 output | c97ef00ac3249b59e9f8a498970675ded78f8f2d84bc55a46652ec4920a6c696 |
| Exploratory E3 output | f89eb5213d05aef754c857236742f3c282d08f591f0d3a8d5af819f2eb5bf98f |

**Exploratory scripts and outputs.** Each exploratory analysis has a script and its full output in the `posthoc_exploratory/` folder of the PNP-RW1 exploratory archive, which is included in the accompanying public evidence release (folder PNP-RW1/IT1 of the bcot-research-commitments repository). The release also contains the two unchanged inputs the scripts read: the runner record and the frozen analysis output. Those make the exploratory analyses reproducible from public data. They do not by themselves make the confirmatory run reconstructible, which also needs the frozen source, seed, beacon and environment. Each script reruns from the frozen record and reproduces its recorded output byte for byte (checked 6 Nov 2026, and again independently by a reviewer). The README marks the original E1 output as superseded by E1r and kept for traceability only. The archive's SHA-256 is 1e064e64c68184a99896b2970d8dd24f0412376133a36615bdcac1b840d643b0.

| Exploratory file | SHA-256 |
| --- | --- |
| E1 script | 9cbb2ee49070d0ff8a16fdb2bd3aca62f81bf18e3d5e8f0d392d1ed16bf73d82 |
| E2 script | 9eacdecdea2b973fb0063e002ae6bf018cfc16b793566af709502f9b12cc3fc6 |
| E3 script | ca6f3b2793df57be0f497cdd9e44cb84bbb229a3d69ee8db7e1beff981f9364d |
| E1r/E2r script (review corrections) | a3029acfe26e9a5efac91692ecab184c6ed29ce7bfb1ed6da3ed419ef1985b92 |
| E1r/E2r output | b5c7a5776c6fea68ddde75591bbe8dfcd6a5076b0b4bea76bbb2f9732b04c0da |
| Folder README | 7102d3dde7b7a548d396342719719441941f6aa9b8c7ee56b41eb33c79facab8 |

**Public commitment:** [GitHub Issue #1](https://github.com/rumplepuff/bcot-research-commitments/issues/1) in [bcot-research-commitments](https://github.com/rumplepuff/bcot-research-commitments), holding the [seed commitment record](https://github.com/rumplepuff/bcot-research-commitments/blob/main/PNP-RW1_IT1_seed_commitment.txt), created 2026-11-03T00:07:43Z. **Beacon:** drand quicknet, chain 52db9ba70e0cc0f6eaf7803dd07447a1f5477735fd3f661792ba94600c84e971, round 33706040, randomness fd01224ba35b6d8d923b02fe12b20f666624ead183a8e42f8567b84126e3ac04. **Effective master seed:** 3055292229349186027.

The last digit of the two CI bounds differs between the analysis and the reconstruction (…833 against …835, and …302 against …305). This comes from adding floating-point numbers in a different order. It falls within the comparator's tolerance and is recorded in custody.
