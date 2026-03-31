# AGENTS.md

## Table Of Contents

- [Repo Context](#repo-context): what this repository is, and what it is not.
- [Start Here](#start-here): the shortest file set that explains startup, query execution, tool contracts, and state.
- [Major Subsystems](#major-subsystems): where to look for commands, tools, UI, services, state, and remote execution.
- [Repo-Specific Patterns](#repo-specific-patterns): behaviors that differ from a normal TypeScript CLI repo.
- [Task Routing](#task-routing): which files to open first for common change types.
- [Supporting Docs](#supporting-docs): where the generated deeper documentation lives.
- [Custom Instructions](#custom-instructions): preserved manual section for repo-specific conventions.

## Repo Context
Tags: provenance, extraction, caveats

This repo is an unofficial extracted snapshot of `@anthropic-ai/claude-code` `2.1.88`, not an upstream development checkout. The important consequence is that `src/` is good for navigation and code reading, but it is missing normal authoring metadata such as `package.json`, lockfiles, CI config, and at least one central recovered type file (`src/types/message.js` is referenced widely but absent from the tree).

Top-level repo contents:

- `src/`: recovered TypeScript and TSX source tree
- `artifacts/original/`: original package tarball
- `artifacts/extracted/`: zip of the recovered source tree
- `scripts/extract-sources.js`: repo-specific extraction helper for `cli.js.map`
- `.agents/summary/`: generated architecture and navigation docs

## Start Here
Tags: entrypoints, navigation, startup

Open these files first for most tasks:

- `src/entrypoints/cli.tsx`: fast-path CLI dispatch and special modes
- `src/main.tsx`: main runtime coordinator
- `src/entrypoints/init.ts`: config, proxy, telemetry, policy, remote settings, and LSP bootstrap
- `src/setup.ts`: cwd, hooks, worktree, tmux, and session setup
- `src/screens/REPL.tsx`: main interactive UI and turn loop
- `src/QueryEngine.ts`: reusable conversation engine
- `src/query.ts`: core query and tool loop
- `src/Tool.ts`: shared tool contract, permission context, and tool-use context
- `src/state/AppStateStore.ts`: primary runtime state model
- `src/Task.ts`: task lifecycle primitives and IDs

## Major Subsystems
Tags: subsystems, ownership

- `src/commands/` and `src/commands.ts`: slash commands, local commands, review/session/plugin flows.
- `src/tools/`, `src/tools.ts`, and `src/Tool.ts`: model-callable tools, schemas, permission context, MCP/resource tools, shell/file tools, and agent/task tools.
- `src/services/`: API, MCP, analytics, compacting, LSP, policy, remote-managed settings, memory, and other integration-heavy code.
- `src/components/`, `src/hooks/`, `src/ink/`, and `src/screens/`: React and Ink terminal UI.
- `src/state/`, `src/Task.ts`, and `src/tasks/`: `AppState`, background tasks, task focus, and foreground or background execution state.
- `src/bridge/`, `src/remote/`, and `src/server/`: bridge mode, remote sessions, direct-connect flows, and reconnect logic.
- `src/skills/` and `src/plugins/`: markdown and frontmatter-driven extensibility plus bundled plugin scaffolding.
- `src/utils/`: a large domain-heavy utility layer for permissions, prompts, session storage, git context, worktrees, telemetry, plugin loading, and prompt processing.

## Repo-Specific Patterns
Tags: patterns, deviations, warnings

- Bun feature flags are everywhere. `feature('...')` plus lazy `require()` calls are part of the architecture, not cleanup debt.
- The same tool abstractions power the REPL, the query engine, and the MCP server.
- Skills and many plugin commands are loaded from markdown and frontmatter, so not every extension point lives in TypeScript registries.
- Input handling is split deliberately: `processUserInput` normalizes prompt text, hooks, attachments, and slash commands before `QueryEngine` and `query.ts` take over.
- The codebase is message-centric, but the canonical recovered message-type file is missing; be cautious when changing anything that depends on exact message unions.
- Some recovered files appear transformed rather than hand-authored. Use them for behavior and dependencies, not for style precedent.

## Task Routing
Tags: routing, common-tasks

- UI or REPL behavior: start with `src/screens/REPL.tsx`, then `src/components/` and `src/hooks/`.
- Slash command behavior: start with `src/commands.ts`, `src/commands/<name>/`, and `src/utils/processUserInput/processSlashCommand.tsx`.
- Tool behavior: start with `src/tools.ts`, `src/Tool.ts`, `src/services/tools/`, and the specific `src/tools/<ToolName>/` directory.
- Prompt-to-model flow: start with `src/utils/processUserInput/processUserInput.ts`, `src/QueryEngine.ts`, and `src/query.ts`.
- State or task behavior: start with `src/state/AppStateStore.ts`, `src/Task.ts`, and `src/tasks/`.
- MCP bugs or integrations: start with `src/services/mcp/`, `src/entrypoints/mcp.ts`, and `src/services/mcp/types.ts`.
- Remote, bridge, or background-session work: start with `src/bridge/`, `src/remote/`, `src/server/`, and `src/tasks/`.
- Memory or injected context issues: start with `src/context.ts`, `src/memdir/`, and `src/utils/claudemd.js`.

## Supporting Docs
Tags: generated-docs, cross-reference

Deeper generated documentation lives under `.agents/summary/`. Use `.agents/summary/index.md` first, then open the focused doc you need:

- `.agents/summary/codebase_info.md`
- `.agents/summary/architecture.md`
- `.agents/summary/components.md`
- `.agents/summary/interfaces.md`
- `.agents/summary/data_models.md`
- `.agents/summary/workflows.md`
- `.agents/summary/dependencies.md`
- `.agents/summary/review_notes.md`

## Custom Instructions

<!-- This section is maintained by developers and agents during day-to-day work.
     It is NOT auto-generated by codebase-summary and MUST be preserved during refreshes.
     Add project-specific conventions, gotchas, and workflow requirements here. -->
