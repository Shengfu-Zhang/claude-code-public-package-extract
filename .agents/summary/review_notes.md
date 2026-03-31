# Review Notes

## Review Scope
Tags: review, consistency, completeness

This review checks the generated documentation against the recovered source snapshot for:

- internal consistency across the generated documents
- completeness relative to the code that is actually present
- explicit gaps caused by extraction limits or missing source-language coverage

## Consistency Check
Tags: findings, consistency

### Finding 1: canonical message-type source is missing from the extracted tree

The generated docs consistently refer to a shared message model, but the canonical source file imported as `src/types/message.js` is not present under `src/types/` in the recovered snapshot. This affects exact reconstruction of:

- the full `Message` union
- assistant and user message variants
- progress and attachment message shapes
- some log and transcript serialization details

Impact:

- architecture and workflow descriptions remain valid
- exact field-level data-model details for messages remain partially inferred

### Finding 2: the snapshot is source-like but not a normal authoring checkout

Some files, notably `src/state/AppState.tsx`, look like transformed output with React compiler-runtime artifacts rather than clean hand-authored source. This means:

- the repository structure is reliable for navigation
- coding-style conclusions from transformed files are less reliable than conclusions from hand-authored TypeScript modules

### Finding 3: dependency and build documentation cannot be authoritative

No `package.json`, lockfile, workspace file, or CI configuration was found in the snapshot. As a result:

- dependency categories can be inferred from imports
- exact versions and official build commands cannot be confirmed from local source

## Completeness Check
Tags: findings, completeness

### Covered well by the generated docs

- repo provenance and extraction context
- entrypoints and runtime surfaces
- command, tool, service, state, and remote subsystem boundaries
- MCP and SDK-related interfaces
- startup and prompt-execution workflows

### Partially covered because the source is incomplete

- canonical message-type definitions
- exact build and packaging pipeline
- precise plugin marketplace or bundled-plugin contents
- some referenced generation scripts or build-time helpers that are mentioned in comments but absent from the recovered tree
- native or unpublished internal components referenced only indirectly

### Not covered because the source is absent

- package-manager metadata
- CI automation configuration
- non-TypeScript source trees for native helpers or vendor components

## Language Support Limitations
Tags: language-gaps, support-boundaries

This documentation run was strong on TypeScript, TSX, JavaScript, and Markdown because those are the languages present in the recovered tree.

Gaps specifically caused by missing language coverage in the repository snapshot:

- No Python, Go, Rust, Java, or C/C++ source was available for analysis.
- Shell and PowerShell behavior can be described only from the TypeScript that invokes them, not from companion source trees in those languages.
- Native-binary behavior from the original package artifacts cannot be reverse-engineered from this TypeScript snapshot alone.

## Recommendations
Tags: recommendations, maintenance

1. Treat `src/types/message.js` as an unresolved extraction gap and verify against original package artifacts before making message-schema refactors.
2. Use `src/entrypoints/cli.tsx`, `src/main.tsx`, `src/screens/REPL.tsx`, `src/QueryEngine.ts`, and `src/query.ts` as the primary code-navigation spine.
3. When documenting or editing dependency-sensitive code, verify imports locally because manifest data is absent.
4. When a feature appears behind `feature('...')`, confirm whether the target session or build actually enables it before assuming the code path is live.
5. Use `.agents/summary/index.md` as the primary AI-assistant context file and pull in `codebase_info.md` or `review_notes.md` whenever missing-file or provenance questions matter.
6. Keep future refreshes conservative about “official” build commands or package versions unless authoritative metadata is added to the repo.
