# Type Info Export for Roc

**Status:** Research / investigation phase
**Filed:** 2026-04-11
**Origin:** Quine-to-Roc port project — need to generate machine-readable error-type documentation from inferred Roc signatures

## One-sentence description

Investigate and potentially contribute a Roc compiler subcommand (e.g., `roc explain` or `roc types --format=json`) that emits machine-readable type information for public signatures, enabling documentation generation, IDE tooling, and type-directed metaprogramming.

## Problem

Roc has a powerful type inference system with open tag unions (`[Foo, Bar]_`) that lets function signatures stay terse while the compiler tracks the complete inferred set behind the scenes. This is idiomatic for error handling — each operation returns `Result x _` and callers get precise per-operation error types without manual enum maintenance.

But the compiler's knowledge isn't exposed to external tooling. If you want to:

- Generate documentation listing all possible errors an API can return
- Build IDE tooling that shows inferred types on hover without re-running the compiler
- Validate that public signatures match expected contracts in CI
- Export types for use by code generators

…you're stuck. `roc docs` generates HTML documentation but it's unclear whether it resolves wildcard `_` to concrete inferred types, and there's no JSON output mode to feed into downstream tools. The canonical Rust equivalent is `cargo check --message-format=json`, which every Rust tooling project depends on.

## Key Documents

- [SPEC.md](SPEC.md) — Investigation plan (TBD, written next)
- [PLAN.md](PLAN.md) — Phase tracking (TBD)
- [research/](research/) — Findings and experiments
- [analysis/](analysis/) — Raw findings

## Origin Context

This plan originated in the Quine-to-Roc port ([github.com/aRustyDev/quine-roc](https://github.com/aRustyDev/quine-roc)) during Phase 2 brainstorming on 2026-04-11. The hybrid approach chosen for Quine's persistence interface was:

- **Public API:** Explicit signatures (stable contracts, readable in source)
- **Private implementation:** Wildcard `_` (open tag union inference)

The brainstorming surfaced that with this design, there's no way to generate a centralized "all possible errors" document without either re-declaring the types or manually keeping docs in sync. That friction hits every Roc project with a public library API — this isn't Quine-specific.

## Success Criteria (tentative — will be refined during investigation)

- [ ] Determine what `roc docs` currently does with wildcard signatures
- [ ] Determine whether the compiler has any internal machine-readable type output
- [ ] Document the gap precisely enough to file an issue / discussion upstream
- [ ] If feasible, prototype a minimal `roc explain` or `roc types --json` subcommand
- [ ] Measure the cost of exposing the internal type environment to external tools

## Non-Goals

- Not trying to rewrite Roc's type system
- Not trying to add new language features
- Not designing a full LSP — just a machine-readable type info export
- Not blocking the Quine port on this contribution landing upstream

## Related

- Quine-to-Roc Phase 2 spec: `quine-roc/.claude/plans/quine-roc-port/refs/specs/phase-2-persistence.md` (when written)
- Roc abilities exploration: `quine-roc/.claude/plans/quine-roc-port/logs/feat/001-explore-roc-abilities-for-persistence.md`
