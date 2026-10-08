# Changelog

## bbd3013 — Adapt scheduling proof from three to four machines

This is a reading guide to the adaptation: what each edit changes, why it is needed in the existing development, and which later arguments depend on it. Upstream names are retained to make comparison easier. Numerical runtime bounds describe what this proof accounts for, rather than optimal exponents.

#### [DynamicProgram.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-ce43d6278415db6a7db06c6ca8cd5c1cf46a7e0c6cf23df39a80117c175d0d46)

A DP state splits a job set into left, middle, and right parts, with the middle representing one full time slot. Requiring four middle jobs also changes the cardinality calculation used to show that transitions make progress.

#### [EffectiveFamily.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-3554f4da6c2237c04fa3a06318320f1472ea3f4a931dd75873ef27a7ea7c784f)

Adds a fourth nested loop to enumerate candidate boundary sets, and extends the membership proof to show that every four-element set appears. The enumeration bound becomes quartic; the original arithmetic tactic still proves the resulting alphabet bound without a rewrite.

#### [FamilyCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-78ae72bf65a2f31d92523d39de0b8b363332eb7822225d136b4c7ad6f87cded6)

The boundary alphabet now has a quartic size bound, so assigning atoms to a description with K leaves has degree 4K rather than 3K. The assignment and family-construction cost proofs are updated to account for that larger enumeration while keeping the existing K = 10000 allowance.

#### [FamilyRoutines.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-3abb9b9f07b31820b640ac9263e26feeb45233f842b8eb46b4b0249cf6ba0463)

Implements the four-element enumeration in the executable routines by adding a tuple component, a membership comparison, and another Cartesian product. These changes keep the compiled computation aligned with the mathematical enumeration in EffectiveFamily.lean.

#### [FrameInvariants.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-356d32580e74e01c36e0e43e888b1bf03740fd6438e5dc7d5ae5338b83674102)

Extends the slot-cardinality proof with a fourth position and proofs that it differs from the other three, retaining the original proof structure. Time bounds and reversal formulas also use the new slot width so later arguments refer to the same layout.

#### [GlobalBoundaries.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-71ea63126b6cf5bcc0693b7622e0d28b05c1290d42d9ea9bb23038c08faab0f7)

The type named Triple now contains sets of exactly four jobs, because an actual boundary represents a full slot. Its historical name is retained to keep the diff focused on the mathematical change.

#### [HierarchyCompleteness.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-49968b9f515f0763b9031483c7cbcc2e39e22d8de2d9a0b1ab29ec643d170ac3)

Updates boundary coordinates, time ranges, and reversal formulas throughout the construction of a hierarchy from a full layout. These substitutions carry the four-job slot convention through the completeness argument without changing its recursive structure.

#### [HierarchySoundness.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-06205d70e2481829806351ec24ca998671e8c55b298aec93f2b6606f1e7c8e58)

Updates full blocks to allow four jobs per time slot and to contain four times their length in jobs. The slot-embedding proof uses Fin T × Fin 4, providing enough distinct positions when a slot contains four jobs.

#### [Main.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-e72cd3f0725d25aca0d8c6eca00ebfdef8e678286bad9645bc2a59d638ac0c0a)

Carries the revised request cost through the existing compilation and input-length bounds, changing the final exponent to 200022. The theorem assembly is otherwise unchanged: the exponent is a sufficient upper bound established by this accounting, not an exact running time.

#### [MatrixCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-6fc25ba46b08a7f2b09fb2803e5f8219302ecf79a810fb400fed7950fa00c93c)

Combines the revised state-generation, marking, and finishing bounds into the cost of solving for one job set. The resulting degree 160011 is then available to the outer layer calculation; this file changes cost accounting rather than solver behavior.

#### [Model.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-464339e18ef170bc5fa24bfb35d4640b67e21d656430e873df30aa8682873ad5)

Changes Feasible to allow at most four jobs at each time and changes the runtime exponent in ReleaseTheorem from 150020 to 200022. This updates the specification to be proved; the definition change alone does not establish an algorithm or its correctness.

#### [NodeCertificates.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-28b3f57dd58e5ea7401e50a5d99df98d74ca24d2cf4cdc6ad783de69dbb80545)

Node and edge certificates carry time coordinates, list-length limits, and bounds on the size of set descriptions. These fields and their construction proofs are updated for four-job slots, six surviving list entries, and the larger description budgets established in the supporting lemmas.

#### [OptimumCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-5a0b3a7a5d27f01f4c6ba3f2b6a91b90156814461081cd5da363c89d50ee0259)

bestAttempt still tries the next deadline, but its cost proof now inherits degree 200012 from scheduleMatrix. Iterating the search up to n times gives degree 200013; the optimization strategy itself is unchanged.

#### [Padding.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-502ebe858644a241a5268ccd90f5cf704627ca02dae2db35dc3d3685e7adc56f)

Uses 4T positions so unused capacity can be filled with dummy jobs before applying the full-layout machinery. The equivalence proof works in both directions, connecting the public feasibility predicate to the padded representation, and the necessary size bound becomes n ≤ 4T.

#### [PriorityUpdates.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-4f9709bfc849d733ccd7d8d28b4740e06aa6a5befad8712eefe23738a2dbf266)

Propagates the larger description budgets through priority changes and switching constructions. The union allowance remains 195 because six entries of at most 29 leaves each still fit: 6 × 29 = 174.

#### [ProgramRoutines.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-5549641e92dfe77fa830abbfbe3aac7c1ede01426d594d0b19f17a0d52cd22c4)

Changes the executable middle-set cardinality test to compare against four. Updating the logical DP definition without this check would leave the routine testing the old slot size.

#### [RankCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-86036738be795a59827b77a8953d05d8e8bcd3219b1f82d31737e7e2677f2f34)

Updates the enumeration expression used in the runtime analysis to match the additional Cartesian product in FamilyRoutines.lean. The cone-list length bound also becomes quartic because cones are generated from four-job boundary sets.

#### [RequestBounds.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-9a52081b67f38cdb47cb2af9e262970f5e8a4200c9cd0c1f27db072f7c61040a)

Updates the cost of answering a deadline query or an optimization query using the revised solver bounds. The answer routines retain their existing behavior; their proofs must accommodate degree 200012 for scheduling and 200013 for optimization.

#### [RequestCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-ed3dc60dacb633b4725099e46fbbc337243bf6b68024665f807e50664895198e)

Raises the complete request-answer cost bound to degree 200013. This passes the revised answer cost through the existing request-processing proof so Main.lean can use it.

#### [RunCertificates.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-1c746d1b1e63c74bb2e7c198fbe61d2f2a3e972ded3c24297856b52d2c3dc702)

The existing injection argument bounds surviving injection entries by the boundary size; four boundary jobs plus two special entries gives a list bound of six. This enlarges the flattened-union budget from 20 to 24 and the first-base and entry budgets from 24/25 to 28/29.

#### [ScheduleBounds.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-3a8042dc1a363e6202560f16b29ab33ddae78c2346533c097eceb243ae593993)

Updates schedule-value and encoded-volume estimates to reflect 4T padding, including the resulting 6T → 8T coefficients in volume bounds. The arithmetic routine cost proof now analyzes (n+n)+(n+n), matching the changed implementation.

#### [ScheduleRoutines.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-09f6bbc15f9c68155740328b5b81bd68e9c793f3a8bc680729ad8bcc2f0930d6)

Changes the routine historically named triple from computing (n+n)+n to computing (n+n)+(n+n). Padding sizes and dependent types are updated alongside it so the executable solver builds and restricts schedules over the intended 4T jobs.

#### [SchedulingAlgorithm.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-ab25fa4549117c0f223c2d1ca7e027209c39ef2aaa45f03071839d520cb0d50c)

Uses 4T padding in the solver and in the proofs that connect its output to Feasible. Updating completeness matters as well as soundness: a solver restricted to three machines could return valid four-machine schedules while still rejecting instances that need the fourth machine.

#### [SchedulingCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-93b8d15573c546830b599b922342e4ab70e220a605dab1a57fee899355a68703)

Combines a degree-160011 cost per family member with family-size degree 40000, giving degree 200011 per DP layer. Iterating up to n layers gives 200012, which is then carried through padding and schedule extraction.

#### [SeparatorWalk.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-fc555bbcdf20520b888a6b522d70f6f0c916fe921e17449b16e2f3a3b92db531)

Changes the layout time formula to group consecutive positions in fours and requires a full layout to have a multiple of four jobs. This is the slot convention consumed by the later boundary and hierarchy proofs.

#### [SolverCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-3066a9b7dabbacf3efac1a08e7f0299afb57b357453a991ee4b973cdbbc8e7bd)

Updates the table-length assumption from degree 30000 to 40000 and propagates it through lookup, transition, and finishing costs. These bounds cover work on the larger family of sets; the routine definitions are unchanged here.

#### [SplitPaths.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-96bad585932ac6e1bcf382c3686831809d111062127beb517205f952e5be88e0)

Changes the slot horizon used when constructing split vertices and checking separator boundaries. The updated ranges keep those split arguments consistent with layouts whose positions are grouped in fours.

#### [StateBounds.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-ada92985e2c2ab0d55edff6128ac0b4fab423934a2f9612324a2e24579cb60c2)

Changes the boundary-list and alphabet length degrees from 3d to 4d, and the family length degree from 3Kd to 4Kd. At K = 10000 and d = 1, this produces the degree 40000 used throughout the later cost proofs.

#### [TableCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-6f43d6720968c07144175ee19866e8babf41064d1afbe761cb8dd016e042d00a)

Accounts for table-size degree 40000 and state/marking-list degree 40004 in the expansion routines. The resulting expansion bound is 160010, and up to n marking iterations raise it to 160011, supplying a key input to the solver cost.

#### [TransitionCosts.lean](https://github.com/drbh/scheduling-proof-exploration/commit/bbd30137258b15d8c5caedd035d72e8e8a875dbd#diff-9f7ff7773655c8fd24c7f937b6d5d1929d040978b9d802efcdebb5f854867cb9)

Updates the cost of forming candidate pairs and filtering them into compatible DP states. The larger family and four-job boundary enumeration raise the candidate-generation bound to 80022 and the state-construction bound to 160010.
