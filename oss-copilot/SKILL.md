---
name: oss-copilot
description: >
  OSS contribution co-pilot for Rudra Dudhat. Use when the user wants to find open source issues
  to contribute to, pick a repo for a PR, boost their OSS profile, or run a full
  end-to-end OSS contribution session (discover issue, write code, submit PR). Triggers on: oss,
  open source, find issues, what should I contribute to, OSS session, /oss-copilot, or any mention
  of contributing to GitHub repos in the LLM security, agentic AI, observability, eval, or backend
  AI infrastructure space.
---

# OSS Co-Pilot

Rudra's end-to-end OSS contribution assistant. Primary niche: **LLM security — agentic attack surfaces** (MCP tool poisoning, agent memory poisoning, multi-agent attack chains). Expanded scope: **any repo that builds job-readiness** for target companies (Portkey.ai, Langfuse, Arize AI, Palantir, Scale AI) — including LLM observability, eval frameworks, agent orchestration, production AI infrastructure, and agentic backend tooling. Target: build a visible, technically credible OSS track record before graduation (2028).

## Workflow

Three phases, always in this order:

**Phase 1 → Discover** — Search GitHub for the single best issue to work on right now.  
**Phase 2 → Pick** — Present one recommendation with full context. Rudra confirms or asks for another.  
**Phase 3 → Execute** — Full session: orient → env check → explain codebase → build → pre-PR checklist → post-push.

At session end: auto-update `oss/CLAUDE.md` status table + append to `session_log.md`.

---

## Phase 1: Discover

Search GitHub fresh every session — do NOT rely on the tracked repo list in oss/CLAUDE.md.

Run multiple `gh search issues` queries in parallel — see [references/issue-search.md](references/issue-search.md) for the full query list and high-value repo table.

Quick pre-filter (drop anything that fails these before spawning agents):
1. Issue is open
2. Repo has been active in the last 6 months
3. Not labeled `closed`, `duplicate`, `wontfix`

For every candidate that passes the pre-filter, **spawn one Issue Verification Agent per candidate** (run in parallel). Each agent does a 4-layer check:

- Layer 1: assignee + labels
- Layer 2: full comment scan for claiming language, code snippets, bot assignments
- Layer 3: linked PRs (open + merged) + GitHub timeline cross-references
- Layer 4: commit history scan + spot-check if the feature/fix already exists in the codebase

The agent returns: `VERDICT: CLEAR / CLAIMED / UNCERTAIN` with a one-line reason.

Full agent prompt and all `gh` commands are in [references/issue-search.md → Issue Verification Agent](references/issue-search.md).

---

## Phase 2: Pick

Present **one** recommendation. Format:

```
Repo:      [org/repo] (N stars, last active: date)
Issue:     #NNN — [title]
Why:       [1 sentence on job-readiness fit — how it builds skill relevant to Portkey/Langfuse/Arize/Palantir/Scale AI]
Effort:    [S / M / L — based on scope of change required]
Signal:    [why this PR will look good to target companies]
Start:     gh repo fork [org/repo] --clone
```

If Rudra says "next" or "another one" — give the next candidate, no explanation needed.

---

## Phase 3: Execute

Once Rudra picks an issue, run the full session flow in order.

See [references/session-flow.md](references/session-flow.md) for the complete protocol.

Quick reference:

1. **Orient** — read oss/CLAUDE.md, check what's in progress
2. **Env check** — `git config user.name` must be `Rudra Dudhat`, email `contact.rdudhat@gmail.com`
3. **Upstream sync** — fork + clone if new repo; fetch upstream + merge if existing
4. **CONTRIBUTING.md** — read it completely, 100%, every time. Non-negotiable. See session-flow.md Step 4a.
5. **Explain codebase** — analogy-first, identify the one file to touch
6. **Build** — write real code matching repo style; write tests if the repo has tests
7. **Pre-PR checklist** — see session-flow.md
8. **QA checklist** — invoke `qa-checklist` skill so Rudra can review everything before push
9. **Push + PR** — conventional commit, PR description with summary + test plan + paper/issue links
10. **Post-push** — monitor CI, fix failures, respond to reviewers
11. **Teach** — run the post-session teaching phase using AI notes format (see session-flow.md Step 11)

---

## Session End (always)

Auto-update without asking:

1. **oss/CLAUDE.md status table** — update Status + Branch + PR columns for the repo worked on
2. **session_log.md** (create if missing, append otherwise):
   ```
   YYYY-MM-DD | org/repo | what was built/changed | next action
   ```

Session log lives at: `c:\Users\rocki\OneDrive\Desktop\altagic\oss\session_log.md`
Status table lives at: `c:\Users\rocki\OneDrive\Desktop\altagic\oss\CLAUDE.md`

---

## Hard Rules

- **No `Co-Authored-By: Claude`** in any commit. Ever.
- **No placeholder code.** Write real implementations or write nothing.
- **No extra scope.** Fix the issue. Nothing else.
- **Commits in Rudra's voice:** conventional commits, lowercase, imperative present tense (`feat: add webhook poisoning probe`, not `Added webhook poisoning probe`).
- **Production systems = read-only** unless Rudra explicitly says otherwise.
