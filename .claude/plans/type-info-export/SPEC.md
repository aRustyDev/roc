# Type Info Export — Investigation Specification

## Purpose

Produce a thorough investigation of Roc's current type information output capabilities, identify exactly where the gap lies between "the compiler knows inferred types" and "external tools can consume them," and — if a meaningful gap exists — design a minimal contribution that closes it.

This is a **research spec**, not a feature spec. The deliverable is knowledge and (optionally) a prototype, not production code. If investigation reveals the gap is already closed or trivially workable around, the plan ends with a short write-up. If the gap is real and significant, the plan ends with an upstream issue/RFC/PR.

## Goals

- Map the current state of Roc's type information output surfaces
- Identify which user needs are met and which aren't
- Locate the compiler internals where the relevant data already exists
- If feasible, produce a minimal working prototype of the proposed output mode
- Write up findings in a form suitable for upstream discussion

## Non-Goals

- No production-ready compiler patch
- No language changes
- No design for a full LSP
- No Quine integration work (Quine proceeds with its hybrid approach independently)

## Scope

### Phase R-1: Baseline Exploration

Answer these questions experimentally by writing small Roc programs and observing compiler behavior:

1. **Does `roc docs` resolve wildcard signatures?** Write a function `f : a -> Result b _` and run `roc docs`. Inspect the HTML output — is the `_` shown literally or expanded to the inferred tag set?
2. **What does `roc check` output for wildcard types?** Does the verbose or debug output include the inferred concrete union?
3. **Does `roc check --help` hint at any machine-readable output modes?** Flags for JSON, LSP, debug-info, etc.
4. **What does the Roc Language Server (`roc_language_server`) expose?** LSP `textDocument/hover` typically returns the inferred type — is this enough for our use case if we drive it via a script?
5. **Are there hidden debug commands?** Try `roc explain`, `roc types`, `roc inspect`, and similar — do any exist?

Deliverable: `analysis/findings/r1-baseline.md` — factual report of what Roc does today.

### Phase R-2: Compiler Internals Mapping

Answer these questions by reading compiler source code (Roc is Zig/Rust; current versions are moving from Rust to Zig):

1. Where in the compiler does type inference produce the final concrete type for a function? Locate the module / function.
2. Where are inferred types stored after inference completes? In-memory representation.
3. Is this type environment written to disk at any point (e.g., incremental compilation cache)?
4. Is there a clean seam at which the type environment could be serialized to JSON / CBOR / similar?
5. What would be the minimal diff to add a new subcommand that walks the type environment and prints it?

Deliverable: `analysis/findings/r2-compiler-internals.md` — annotated pointers to the relevant compiler code.

### Phase R-3: Gap Analysis

Combine findings from R-1 and R-2 to answer:

1. What tools does Roc have today that partially address the need? (`roc docs`, LSP hover, etc.)
2. What's missing, precisely? (A specific data format, a specific subcommand, a specific public API?)
3. Are there existing upstream issues or discussions about this? Check the Roc repo and Zulip.
4. Is the contribution shape:
   - **Option A:** A new subcommand (`roc explain`, `roc types --json`)?
   - **Option B:** A new flag on an existing subcommand (`roc check --emit-types=json`)?
   - **Option C:** An extended `roc docs` mode that resolves wildcards?
   - **Option D:** A library API that third-party tools can link against?
5. What's the rough estimate of implementation complexity?

Deliverable: `analysis/findings/r3-gap-analysis.md` — the contribution shape and complexity.

### Phase R-4: Prototype (Optional, If Gap Is Real)

If R-1 through R-3 conclude that the gap is real and the contribution shape is clear:

1. Fork work branch from upstream main
2. Implement the minimum viable version of the chosen option
3. Test with a small Roc program including wildcard signatures
4. Write up usage and example output in `research/results/r4-prototype-demo.md`

Deliverable: A working prototype on a feature branch, not necessarily merged.

### Phase R-5: Upstream Engagement

Depending on R-3/R-4 findings:

1. **If no gap:** Write `analysis/results/findings-summary.md` explaining what works and close the plan
2. **If gap is small:** File an issue upstream with findings and example use case
3. **If gap warrants code:** File an RFC / discussion upstream, then potentially open a PR with R-4's prototype
4. **In all cases:** Write up the investigation for the Quine-to-Roc port's FR 002 log so the Quine project has the latest picture

Deliverable: Upstream issue/RFC/PR link(s), plus final write-up.

## Success Criteria

- Each R-phase's deliverable is written and committed
- Findings are specific enough that someone reading them can reproduce the experiments
- If a gap is found, the contribution shape is clearly articulated with rationale
- Quine's FR 002 log is updated with the latest status

## Out of Scope

- Making the Quine project depend on this landing
- Forking or patching Roc beyond the investigation branch
- Designing a full LSP implementation
- Touching the Roc language specification
