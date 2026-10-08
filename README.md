# Three-machine scheduling: upstream baseline

This standalone project contains the unchanged 53 scheduling source files from
OpenAI's `openai/math`, revision `fd4aeeb2ee4fc729c18d98444fed42fd0529eeeb`
(the upstream HEAD retrieved on 2026-10-08):
https://github.com/openai/math/tree/fd4aeeb2ee4fc729c18d98444fed42fd0529eeeb/lean/OAI/Computability/Scheduling

The source directory is copied verbatim, with the upstream `lean/` prefix
removed. The Lean toolchain and license are copied from upstream. The Lake
configuration is narrowed to this library and its Mathlib dependency, pinned
to the same upstream revision. Other mathematics libraries are excluded.

```sh
lake exe cache get
lake build OAI.Computability.Scheduling.Main
```

The entry point is `OAI.ThreeMachine.main_theorem`. This initial commit is the
three-machine baseline; the following commit contains the complete fixed-four
adaptation.
