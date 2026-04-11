# Roadmap

## Phase R-1: Baseline Exploration
- [ ] Run `roc docs` on wildcard-signature example, inspect HTML output
- [ ] Run `roc check --help` and examine all flags for machine-readable output
- [ ] Drive `roc_language_server` LSP interactively and observe `textDocument/hover` for wildcard types
- [ ] Check for hidden subcommands: `roc explain`, `roc types`, `roc inspect`, `roc info`
- [ ] Write `analysis/findings/r1-baseline.md`

## Phase R-2: Compiler Internals Mapping
- [ ] Locate Roc type inference implementation in source tree
- [ ] Locate the type environment data structure
- [ ] Identify serialization hooks or incremental-compile cache format
- [ ] Write `analysis/findings/r2-compiler-internals.md` with code pointers

## Phase R-3: Gap Analysis
- [ ] Combine R-1 and R-2 findings
- [ ] Search Roc repo issues and Zulip for prior discussions
- [ ] Decide contribution shape (new subcommand, new flag, extended docs, library API)
- [ ] Write `analysis/findings/r3-gap-analysis.md`

## Phase R-4: Prototype (optional)
- [ ] Create feature branch for prototype work
- [ ] Implement minimum viable output mode
- [ ] Test with Quine-style example (wildcard signatures + explicit public API)
- [ ] Write `research/results/r4-prototype-demo.md`

## Phase R-5: Upstream Engagement
- [ ] File issue OR RFC OR PR per R-3 recommendation
- [ ] Update Quine FR 002 log with final status
- [ ] Write `analysis/results/findings-summary.md`
