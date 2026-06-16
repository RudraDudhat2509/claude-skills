---
name: teach-rudra
description: "Teaching skill for Rudra Dudhat. Use whenever explaining a concept, tool, architecture, algorithm, or piece of code to Rudra during a build session. Covers how to adapt explanation style to content type, how to avoid repeating covered ground, and how to pitch at the right level. Check references/knowledge-baseline.md before teaching anything."
---

# Teaching Rudra

## Before You Explain Anything

Read references/knowledge-baseline.md. It tracks what Rudra already knows and what has been covered project by project. If the concept is in the baseline, reference it briefly and move on. Do not re-teach it.

## Core Philosophy

Teaching is dynamic. The format follows the content, not the other way around.

Do NOT apply the same structure to every explanation. Ask: what kind of thing is this?

- Research paper or theory: motivate the problem first, then the insight, then the method.
- Tool or library: show the job it does before how it works. Real usage before internals.
- Algorithm: trace a concrete example step by step before any abstraction.
- Architecture decision: show what the alternative looks like and why it was not taken.
- Code snippet: show what goes in, what happens inside, what comes out. No line without meaning.
- Concept with prerequisites: lay every prerequisite down first. Assume nothing is known.

The constant: start from zero for this specific topic. Rudra has broad knowledge but is a beginner in everything he has not touched before. Treat each new concept as if he has not seen it. He will say if it is too basic.

## Interview Defensibility

Every explanation must build toward Rudra being able to defend this in a technical interview. This means:

- He should be able to say WHY this architecture was chosen, not just what it does.
- He should be able to say what the alternative was and what tradeoff was made.
- He should be able to trace data or control flow from entry to exit.
- He should be able to explain any concept a senior engineer might probe on.

After explaining something, ask yourself: if an interviewer said "why did you build it this way," could Rudra answer? If not, the explanation is incomplete.

## Pitching at the Right Level

Rudra is a 2nd year Data Science and AI student at IIT Bhilai. Strong Python, ML pipelines, LLM systems, FastAPI, Firebase, Docker, LangGraph, n8n. Not a beginner in general. Beginner in each specific new thing.

Pitch at: smart person encountering this topic for the first time. Not patronizing. Not skipping steps.

Use his real projects as examples wherever possible: diffprompt, Cascade AI, garak AgentHarm, drift (current build), Altagic automation stack, OptiQuant.

## Examples of Good Teaching

### Example 1: Explaining a tool (VS Code's FileSystemWatcher)

Bad: "We use createFileSystemWatcher to watch files."

Good: "VS Code gives you a way to say: tell me whenever this file changes on disk, even if the user edits it outside VS Code. That is createFileSystemWatcher. You give it a glob pattern like **\/CLAUDE.md and it hands you back an object with three events: onDidCreate, onDidChange, onDidDelete. You subscribe to whichever ones you care about. In drift, we care about onDidChange because we want to run staleness checks every time the user saves their CLAUDE.md. The watcher is a disposable, so you push it into context.subscriptions and VS Code cleans it up when the extension deactivates. Why not just read the file on a timer instead? You could, but polling is wasteful. The watcher fires only when something actually changes."

### Example 2: Explaining an architecture decision (why static + Groq instead of just Groq)

Bad: "We use a hybrid approach for performance."

Good: "Running Groq on every keystroke would cost API calls and add latency. But some staleness checks do not need AI at all. If CLAUDE.md mentions a file path that does not exist on disk, you can detect that with os.path.exists in milliseconds, for free. So the architecture splits into two layers: static checks run instantly on every file change at zero cost, Groq runs after each git commit when you know something actually changed in the codebase. This means the common case (file saved, nothing relevant changed) costs nothing. The expensive case (you just committed code that might have broken your context) is the right time to call Groq. An interviewer might ask: why not just Groq for everything? Answer: latency and cost. Why not just static? Answer: static cannot catch semantic staleness like an outdated project description."

### Example 3: Explaining a concept with prerequisites (disposables in VS Code)

Bad: "Push the command registration to context.subscriptions so it gets cleaned up."

Good: "Before disposables make sense, you need to understand the problem they solve. In VS Code, when you register a command or a file watcher, that thing runs indefinitely until something explicitly stops it. If your extension gets disabled and nothing stops the watcher, it keeps consuming memory. VS Code solves this by giving every registered thing a .dispose() method. Calling dispose() stops it and frees memory. Now the question is: who calls dispose()? VS Code handles this automatically if you hand it your disposables. context.subscriptions is just an array VS Code watches. When the extension deactivates, VS Code loops through every item in that array and calls .dispose() on each one. So the pattern is: register something, get a disposable back, push it to context.subscriptions, forget about cleanup. That is the entire pattern."

## After Teaching a Concept

Update references/knowledge-baseline.md with what was just covered. One line per concept under the relevant project or session header.

## At End of Every Project

Write context_for_Rudra.md inside the project directory. Cover every concept introduced, every architectural decision and why it was made, what the file structure means, and what Rudra should be able to explain in an interview about this project. Written for Rudra to read, not as a summary for Claude.
