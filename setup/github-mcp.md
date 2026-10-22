# GitHub MCP — choose your tool

Use the [official GitHub MCP server](https://github.com/github/github-mcp-server).
Docker must be running for the local-server commands below. Choose a published server
tag from the official releases, set `GITHUB_MCP_VERSION` to it, and record that exact
version in `results/test-session-2.md`. Do not silently switch versions mid-assessment.

Create a fine-grained GitHub token with access to the course repository and **Contents:
read** and **Pull requests: read**. Keep `GITHUB_PERSONAL_ACCESS_TOKEN` in your shell's
environment, never in a committed file. The official remote server with its documented
authentication is also acceptable; record the transport and advertised tool names.

## Codex CLI

Register a local server; `-e NAME` forwards the environment variable without embedding its
value in the command or configuration:

```sh
codex mcp add github -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN -e GITHUB_READ_ONLY=1 "ghcr.io/github/github-mcp-server:${GITHUB_MCP_VERSION}"
```

Restart Codex. Use `/mcp` to inspect the connection and tools. Codex normally stores the
registration in your user configuration; a project `.codex/config.toml` is ignored here.
Never ignore all of `.codex/`: its `agents/` definitions belong in the submission.

## Claude Code

Register the same read-only server in local scope:

```sh
claude mcp add --transport stdio --scope local github -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN -e GITHUB_READ_ONLY=1 "ghcr.io/github/github-mcp-server:${GITHUB_MCP_VERSION}"
```

Restart Claude Code and use `/mcp`. If you configure a project `.mcp.json` manually,
keep it ignored. `.claude/settings.local.json` is also ignored.

## Required exercise and evidence

1. Set `review-target.json` to `mode: "github"`, owner `nebius-academy-templates`, repo
   `go-pr-review-practice`, and the supplied 1–5 mapping.
2. Verify the server advertises `pull_request_read`. Use `method: "get"` for identity and
   head SHA, `get_diff` for changes, and paginated `get_files` when checking coverage.
   Clients may prefix tool names; preserve the actual name in the transcript.
3. Review PR-01, display the complete draft and ask to post. Answer `no`.
4. Save the actual read call and response, review, question, and refusal in
   `results/test-session-2.md`. Mask credentials without removing PR identity or tool names.

Read-only server mode and credentials intentionally prevent publication. The exercise
checks asking and declining; **no comment or pending review is required**. Never replace
an MCP read with `gh`, `curl`, or a local sample and label it an MCP run.

The optional publication operation is `add_issue_comment`, an ordinary PR conversation
comment. `add_comment_to_pending_review` and `pull_request_review_write` belong to a
pending-review workflow and are not required. Publication would require a separate
session with suitable write permissions and explicit approval of the displayed draft
and target. Never approve, merge, or close the PR.

Sources: [server tools and read-only mode](https://github.com/github/github-mcp-server),
[Codex MCP](https://developers.openai.com/codex/mcp/),
[Claude Code MCP](https://code.claude.com/docs/en/mcp).
