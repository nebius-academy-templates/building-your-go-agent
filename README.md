# Building Your Go Agent

Configure a Go PR reviewer in **Codex (CLI or desktop app) or Claude Code**. Choose one tool for the
whole course; you do not need both subscriptions. This repository contains agent
instructions and evidence, not a Go implementation of an agent runtime.

Read [practice-description.md](practice-description.md) for the seven cumulative
practices. The `tool` field in `review-target.json` is initially empty: before Practice 1,
choose `codex` (Codex CLI or desktop app) or `claude` (Claude Code). Neither is selected for you.

| Practice | What you add |
| --- | --- |
| 1. Assembling | Shared system instructions and boundary-test transcript |
| 2. Tooling | Live GitHub MCP read and declined publication |
| 3. Skills | Selective convention skills and explicit review workflow |
| 4. Subagents | Three sequential reviewers and consolidation |
| 5. Memory | Append-only history and ADR-backed findings |
| 6. Observability | Real LangSmith traces and saved span evidence |
| 7. Final | Five-case evaluation, injection resistance, improvement proposal |

## One source, two entry points

- `AGENTS.md`: shared instructions. `CLAUDE.md` imports it.
- `skills/*.md`: shared skill rules/workflow you complete.
- `.agents/skills/*/SKILL.md`: Codex entry points; `.claude/skills/*/SKILL.md`: Claude entry points.
- `reviewers/*.md`: shared reviewer prompts you complete.
- `.codex/agents/*.toml` and `.claude/agents/*.md`: native reviewer definitions.
- `docs/adr/`: team decisions. `results/`: evidence the AI reviewer can read offline.

Skills explicitly load their shared files. No symlinks or generator is required.
Open the actual repository root as a trusted Codex project before starting a new session.
Untrusted folders can suppress project configuration and role discovery.
In the Codex desktop app, open this repository as the project and start a new chat;
use the same `$review-pr` command. Subagent APIs differ between clients/versions: use
named roles when the runtime exposes them, otherwise pass the TOML instructions to
real child agents explicitly, as described in Practice 4. Merely naming a task does
not load a TOML file. A client with no subagent tool cannot complete Practice 4+.
Desktop tracing requires a separate integration check before Practices 6/7; see
[tracing setup](setup/tracing.md). Successful delegation alone does not establish tracing support.
Use `$review-pr 3` in Codex and `/review-pr 3` in Claude Code. The native definitions
are supplied; complete the shared files for your current stage and leave the other
tool's configuration alone. Subagents inherit the main model; record the actual model
and tool/plugin versions in your evidence. Selective loading is tested before subagents.

## Review inputs

The separate [course repository](https://github.com/nebius-academy-templates/go-pr-review-practice)
has PRs [1](https://github.com/nebius-academy-templates/go-pr-review-practice/pull/1),
[2](https://github.com/nebius-academy-templates/go-pr-review-practice/pull/2),
[3](https://github.com/nebius-academy-templates/go-pr-review-practice/pull/3),
[4](https://github.com/nebius-academy-templates/go-pr-review-practice/pull/4), and
[5](https://github.com/nebius-academy-templates/go-pr-review-practice/pull/5).
Each is independent. `samples/pr-0N-*.diff` provides matching local inputs.
Read [fixture-policy.md](fixture-policy.md); leave its intentionally flawed code unchanged.

`review-target.json` starts in `local` mode for Practice 1, before MCP is configured.
Change it to `github` in Practice 2. A local run cannot satisfy a live MCP requirement.
Record the actual PR number and head SHA so later changes do not silently change your input.

See [GitHub MCP setup](setup/github-mcp.md) and [tracing setup](setup/tracing.md).
Keep secrets out of Git and redact credential values from evidence, including tool output.
All learner-authored files already exist. Use LF endings and the exact `results/` paths.

## Assessment

Automated checks run first; an AI reviewer then reads the committed files. It cannot
execute code or open links, including LangSmith links. Save trace IDs, measured values,
relevant span outputs and real tool-call transcripts in the evidence files themselves.
Never substitute an illustrative transcript or estimated token count for an actual run.
