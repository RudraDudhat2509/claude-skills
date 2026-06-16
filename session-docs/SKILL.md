---
name: session-docs
description: Run after every work session to update memory files. Scans what changed and writes immediately — no approval step. Targets: missing decisions, stale file paths, outdated status.
---

# Session Documentation Skill

Run this at the end of every session. The goal is a memory system that is always accurate and never redundant — future sessions should be able to pick up exactly where this one left off without re-reading the conversation.

## What this skill does

1. Scans the session for what's worth saving
2. Identifies which memory files need changes
3. Writes immediately — no proposal, no approval gate

## Step 1 — Scan the session

Go through the conversation and extract only things that fit one of these three categories:

**Outdated status** — something was PENDING and is now DONE, or was assumed working and broke, or a decision was reversed. Status that doesn't match reality is the most damaging type of stale memory.

**Missing decisions** — a non-obvious choice was made and the WHY isn't captured anywhere. If someone reads the code in 3 months, will they understand why this approach was taken? If not, it belongs in memory.

**Stale file paths / function names** — a file was renamed, moved, deleted, or a key function changed its name or signature. Memory that points to a ghost is worse than no memory at all.

Do NOT capture:
- Things that are obvious from reading the code or running `git log`
- In-progress work that isn't settled yet
- Error messages and debugging steps (the fix is in the code)
- Anything already in CLAUDE.md
- Temporary state from this session only

### Graphify context (check every session)

All three altagic codebases have persistent knowledge graphs in `graphify-out/` directories:

| Project | Path | Last known stats |
|---|---|---|
| Color Tool | `Color_tool/graphify-out/` | 452 nodes, 710 edges, 43 communities |
| HR Dashboard | `hr_dashboard/graphify-out/` | 609 nodes, 861 edges, 42 communities |
| Drapeme Pipeline | `drapeme-pipeline/graphify-out/` | 216 nodes, 425 edges, 14 communities |

Each directory contains: `graph.html` (interactive vis.js), `graph.json` (GraphRAG-ready), `GRAPH_REPORT.md`.

If a session built or updated any of these graphs, record the new node/edge counts in memory. If the session made significant structural changes to a codebase (new modules, renamed files, deleted components), note that the graph is **stale** and should be rebuilt with `/graphify <path>` before the next deep codebase question.

**Future sessions: prefer `graphify query "<question>"` over cold file reads for architecture questions.** The graph survives across sessions and provides cross-file context you cannot get from grep alone.

## Step 2 — Identify target files

Memory lives at: `C:\Users\rocki\.claude\projects\c--Users-rocki-OneDrive-Desktop-altagic\memory\`

Read the MEMORY.md index. For each piece of extracted information, identify which existing memory file it belongs in. Only create a new file if the information genuinely belongs to a new category that doesn't fit any existing file.

Read the relevant target files before writing.

## Step 3 — Write immediately

No proposal. No approval gate. Write directly.

1. Update each file in place — overwrite the stale section, preserve everything else
2. Update the MEMORY.md index line if the description changed
3. Output a one-line confirmation: "Updated X files: [list]."

If nothing meaningful changed this session, say so explicitly: "Nothing worth documenting this session." Do not manufacture changes.

## Behavior rules

**Overwrite, don't append.** If a section is being updated, replace it. Don't leave both old and new versions.

**Verify before referencing.** If you're about to write a file path or function name into memory, confirm it still exists in the codebase first.

**One truth per fact.** If the same information appears in two memory files, flag it. Memory should have a single home for each piece of information.

**Dates are absolute.** If the session includes relative dates ("next Thursday", "in 2 weeks"), convert them to absolute dates (YYYY-MM-DD) before writing.

**Short is better.** A memory entry that fits in 2 lines and is correct beats a 10-line entry that's 80% accurate. Compress ruthlessly.
