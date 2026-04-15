---
theme: the-unnamed
title: Agentic Development
info: |
  ## Agentic Development
  Working with AI coding agents as collaborators, not autocomplete.
class: text-center
drawings:
  persist: false
transition: slide-left
colorSchema: dark
comark: true
duration: 35min
---

<div class="flex flex-col justify-center items-center h-full">

# Agentic Development

<div class="text-xl opacity-70 mt-4">
  Working with AI coding agents as collaborators, not autocomplete
</div>

</div>

<div class="abs-br m-6 flex items-center gap-3 leading-none">
  <span class="text-sm opacity-50">Davide Silva</span>
  <span class="opacity-30">|</span>
  <img src="/assets/subvisual-white.svg" class="h-5 opacity-50" alt="Subvisual" />
</div>

---
layout: center
class: text-center
---

# Davide Silva

<div class="text-xl opacity-80 mt-2 flex items-center justify-center gap-3 leading-none">
  <span>Lead Software Engineer</span>
  <span class="opacity-40">·</span>
  <img src="/assets/subvisual-white.svg" class="h-5 opacity-80" alt="Subvisual" />
</div>

<div class="mt-8 opacity-70 text-sm space-y-1">
  <div><a href="https://github.com/DavideSilva">github.com/DavideSilva</a></div>
  <div><a href="https://blog.davidesilva.dev/">blog.davidesilva.dev</a></div>
</div>

---
transition: fade-out
---

# The Problem

<div class="text-xl italic opacity-80 mb-8">
  "I generated code with AI and it came out inconsistent."
</div>

<v-clicks>

- The agent doesn't know your conventions
- You paste the same setup prompt every session
- Without structure, the agent is a junior with no onboarding

</v-clicks>

---
transition: fade-out
---

# The Core Idea

<div class="text-xl mt-8 mb-8">
  Don't just ask AI to generate code.
</div>

<div v-click class="text-xl mb-8">
  <span v-mark.underline.orange>Structure your project</span> so the agent is productive from the very first moment —
  just like you'd onboard a new developer.
</div>

---
layout: center
class: text-center
---

# The Building Blocks

<div class="text-lg opacity-70 mt-4 mb-10">
  Four essentials for a productive agent
</div>

<div class="grid grid-cols-1 gap-3 text-left max-w-md mx-auto">

<div v-click class="flex gap-3"><span class="opacity-40">01</span> Context files</div>
<div v-click class="flex gap-3"><span class="opacity-40">02</span> Slash commands &amp; skills</div>
<div v-click class="flex gap-3"><span class="opacity-40">03</span> Subagents</div>
<div v-click class="flex gap-3"><span class="opacity-40">04</span> MCP servers</div>

</div>

---

# 01 · Context Files

<div class="text-lg opacity-80 mb-6">
  <code>CLAUDE.md</code>, <code>AGENTS.md</code> — the agent reads this on <em>every</em> session.
</div>

<div class="grid grid-cols-2 gap-8 mt-8">

<div>

### Put in

- Stack &amp; architecture at a glance
- Coding conventions
- How to run, test, lint
- Non-obvious gotchas

</div>

<div>

### Keep out

- Secrets &amp; credentials
- Anything derivable from the code
- Ephemeral task notes
- Giant walls of text

</div>

</div>

<div v-click class="mt-8 opacity-70 text-sm">
  Think of it as the README you'd hand a new hire on day one.
</div>

---

# 01 · Context Files — Example

```md
# Project: Checkout Service

Stack: TypeScript · Fastify · Postgres · Bun

## Commands
- `bun dev` — run locally
- `bun test` — run tests (use this before claiming done)
- `bun run lint` — must pass before commit

## Conventions
- Prefer relative imports
- No hex/px values — use Tailwind tokens
- Errors bubble up; don't swallow with try/catch

## Gotchas
- Webhooks are idempotent — always check `event.id`
- Migrations run on deploy, not on boot
```

---

# 02 · Slash Commands &amp; Skills

<div class="text-lg opacity-80 mb-6">
  Reusable workflows the agent can invoke on demand.
</div>

<div class="grid grid-cols-2 gap-8 mt-6">

<div>

### Why

- Stop retyping the same instructions
- Standardize workflows across a team
- Turn tribal knowledge into muscle memory

</div>

<div>

### Examples

- `/commit` — conventional commit message
- `/review-pr` — structured PR review
- `/new-migration` — scaffolded to your conventions

</div>

</div>

<div v-click class="mt-8 pt-6 border-t border-white/10 opacity-80">

**Slash commands** — quick prompt shortcuts you type.
**Skills** — richer units: metadata, scripts, files. The agent picks them up automatically when relevant.

</div>

---

# 02 · A Skill, concretely

````md
---
name: new-migration
description: Use when the user asks for a new database migration.
---

# Creating a migration

1. Run `bun run db:new <name>` to scaffold the file
2. Write the `up` and `down` blocks — both are required
3. Never edit a migration that has already shipped
4. Update `schema.sql` after running it locally
5. Add a test that exercises the new columns
````

<div v-click class="mt-6 opacity-70 text-sm">
  The agent reads the <code>description</code>, decides when to invoke, and follows the steps —
  so every migration in the repo comes out the same way.
</div>

---

# 03 · Subagents

<div class="text-lg opacity-80 mb-6">
  Specialized agents with their own context, tools, and system prompt.
</div>

<div class="grid grid-cols-2 gap-8 mt-6">

<div>

### Why

- Keep the main context clean
- Parallelize independent work
- Right tool for the job — not one generalist

</div>

<div>

### Examples

- `code-reviewer` — reviews a diff against standards
- `explorer` — surveys unfamiliar codebases
- `test-runner` — runs &amp; triages failures
- `security-auditor` — scans for OWASP issues

</div>

</div>

<div v-click class="mt-6 pt-4 border-t border-white/10 opacity-80 text-sm">

**Reach for a subagent when** the task is self-contained · would flood the main context · needs a focused prompt the main agent doesn't carry.

</div>

---

# 03 · A Subagent, concretely

````md
---
name: code-reviewer
description: Review a diff against the project's coding standards.
  Use after a major feature is implemented, before committing.
tools: Read, Grep, Glob, Bash
---

You are a senior reviewer. Your job is to catch issues the author missed.

Focus on:
- Correctness &amp; edge cases
- Security (injection, auth, secrets)
- Consistency with conventions in CLAUDE.md
- Tests that actually exercise the change

Report findings as a prioritized list.
Do not fix anything — just review.
````

<div v-click class="mt-6 opacity-70 text-sm">
  The main agent delegates; the reviewer runs in its own sandbox and returns a report.
  No review noise pollutes the main thread.
</div>

---

# 04 · MCP Servers

<div class="text-lg opacity-80 mb-6">
  A protocol for giving the agent access to external tools &amp; data.
</div>

<div class="grid grid-cols-2 gap-8 mt-6">

<div>

### Why

- The agent can <em>do</em> things, not just edit files
- Query your DB, hit your APIs, read designs
- Same interface across any MCP-aware client

</div>

<div>

### Examples

- **Figma** — pull designs into code
- **Sanity** — query &amp; mutate content
- **Playwright** — drive a real browser
- **Postgres / GitHub / Linear** — you name it

</div>

</div>

<div v-click class="mt-8 pt-6 border-t border-white/10 opacity-80">

**The tradeoff** — each server eats context. More tools ≠ better agent. Install what the project actually needs.

</div>

---

# 04 · Wiring up an MCP server

```bash
# Remote HTTP server
claude mcp add --transport http figma https://mcp.figma.com/mcp

# Same pattern for any remote server
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

---
layout: center
class: text-center
---

# Plan Before You Code

<div class="text-lg opacity-70 mt-4">
  The fastest way to ship the wrong thing is to start typing.
</div>

---

# Brainstorm → Spec → Plan → Code

<div class="grid grid-cols-4 gap-3 mt-6 text-center">

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Brainstorm</div>
  <div class="text-xs opacity-70">Explore intent &amp; requirements <em>with</em> the agent</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Spec</div>
  <div class="text-xs opacity-70">Write the design down. Commit it.</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Plan</div>
  <div class="text-xs opacity-70">Step-by-step implementation, reviewed before code</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Code</div>
  <div class="text-xs opacity-70">Execute against the plan, tracked as todos</div>
</div>

</div>

<div v-click class="mt-10 space-y-3 opacity-90">

- **Brainstorming** surfaces assumptions before they become bugs
- **Plan mode** — the agent proposes, <span v-mark.underline.orange>you approve</span>, then it touches code
- Each step is cheap to redo. The code you'd have to rewrite isn't.

</div>

---

# How much planning?

<div class="text-lg opacity-80 mb-8">
  Match the ceremony to the size of the task.
</div>

<div class="space-y-4">

<div v-click class="flex gap-4 items-start">
  <span class="text-xs px-2 py-1 bg-white/10 rounded shrink-0 min-w-30 text-center">Small fix</span>
  <div class="opacity-90">Just do it. A one-line bug doesn't need a design doc.</div>
</div>

<div v-click class="flex gap-4 items-start">
  <span class="text-xs px-2 py-1 bg-white/10 rounded shrink-0 min-w-30 text-center">New feature</span>
  <div class="opacity-90">Quick plan, skip the spec. Review the steps before executing.</div>
</div>

<div v-click class="flex gap-4 items-start">
  <span class="text-xs px-2 py-1 bg-white/10 rounded shrink-0 min-w-30 text-center">Big refactor</span>
  <div class="opacity-90">Full flow: brainstorm → spec → plan → code. Commit each artifact.</div>
</div>

</div>

<div v-click class="mt-10 text-center text-lg italic opacity-80">
  The bigger the task, the more upfront planning pays off.
</div>

---
layout: center
class: text-center
---

# Putting It Together

<div class="text-lg opacity-70 mt-4">
  One feature, four building blocks.
</div>

---

# Feature from a Figma design

<div class="text-sm opacity-70 mb-4">
  Designer drops a Figma link. "Build this."
</div>

<div class="font-mono text-base bg-black/30 border border-white/10 rounded-lg p-6 mt-6 max-w-3xl mx-auto">

<div v-click class="flex items-baseline gap-3">
  <span class="text-orange-400">$</span>
  <span>"build this Figma design"</span>
</div>

<div v-click class="flex items-baseline gap-3 mt-3">
  <span class="opacity-40">→</span>
  <span>reads <code>CLAUDE.md</code></span>
</div>

<div v-click class="flex items-baseline gap-3 mt-1">
  <span class="opacity-40">→</span>
  <span><code>get_design</code> via Figma MCP</span>
</div>

<div v-click class="flex items-baseline gap-3 mt-1">
  <span class="opacity-40">→</span>
  <span><code>/design-to-component</code></span>
</div>

<div v-click class="flex items-baseline gap-3 mt-1">
  <span class="opacity-40">→</span>
  <span>dispatches <code>code-reviewer</code></span>
</div>

<div v-click class="flex items-baseline gap-3 mt-1">
  <span class="opacity-40">→</span>
  <span><code>/commit</code></span>
</div>

<div v-click class="flex items-baseline gap-3 mt-3">
  <span class="text-green-400">✓</span>
  <span>PR ready</span>
</div>

</div>

<div v-click class="mt-4 opacity-70 text-sm italic text-center">
  No copy-paste. No re-explaining. The project tells the agent how to work here.
</div>

---
layout: center
class: text-center
---

# Pitfalls

<div class="text-lg opacity-70 mt-4">
  Easy ways to sabotage yourself.
</div>

---

# What not to do

<div class="grid grid-cols-2 gap-6 mt-4">

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-2 flex items-center gap-2"><carbon:document class="opacity-60" />The 500-line CLAUDE.md</div>
  <div class="text-sm opacity-70">Nobody maintains it. It rots. Keep it tight — onboarding, not a manual.</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-2 flex items-center gap-2"><carbon:plug class="opacity-60" />MCP buffet</div>
  <div class="text-sm opacity-70">Every tool eats context. More servers ≠ smarter agent. Install what the project needs.</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-2 flex items-center gap-2"><carbon:flash class="opacity-60" />Autocomplete mindset</div>
  <div class="text-sm opacity-70">Skipping structure, then blaming the agent for inconsistency. Onboarding first.</div>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-2 flex items-center gap-2"><carbon:view-off class="opacity-60" />Uncommitted context</div>
  <div class="text-sm opacity-70">Your teammate's agent doesn't know what yours knows. Commit the context files.</div>
</div>

</div>

---
layout: center
class: text-center
---

# Takeaway

<div v-click class="text-2xl mt-10 leading-relaxed">
  Treat your project as the agent's <span v-mark.underline.orange>onboarding doc</span>.
</div>

<div v-click class="text-xl mt-8 opacity-80">
  Start small: one <code>CLAUDE.md</code>, one skill, one subagent.
</div>

<div v-click class="text-lg mt-8 opacity-60">
  The more structure you give it, the more leverage you get back.
</div>

---
layout: center
class: text-center
---

# Tools Worth Knowing

<div class="text-lg opacity-70 mt-4">
  Some I use. Some are on my radar.
</div>

---

# A few worth a look

<div class="grid grid-cols-2 gap-4 mt-4">

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Superpowers</div>
  <div class="text-xs opacity-60 mb-2">Claude Code plugin</div>
  <div class="text-sm opacity-80 mb-2">Ships ready-made skills: brainstorming, planning, TDD, debugging, code review. Drop-in process for working with the agent.</div>
  <a href="https://github.com/obra/superpowers" class="text-xs opacity-70 hover:opacity-100">github.com/obra/superpowers →</a>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Agent Deck</div>
  <div class="text-xs opacity-60 mb-2">TUI · multi-agent control</div>
  <div class="text-sm opacity-80 mb-2">Mission control for many sessions at once: status, session forking, MCP &amp; skill management, git worktrees, Docker sandboxes.</div>
  <a href="https://github.com/asheshgoplani/agent-deck" class="text-xs opacity-70 hover:opacity-100">github.com/asheshgoplani/agent-deck →</a>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Headroom</div>
  <div class="text-xs opacity-60 mb-2">Context compression</div>
  <div class="text-sm opacity-80 mb-2">Sits between agent and LLM, compresses tool outputs &amp; file reads. Fewer tokens, same answers — runs locally as a proxy or SDK.</div>
  <a href="https://github.com/chopratejas/headroom" class="text-xs opacity-70 hover:opacity-100">github.com/chopratejas/headroom →</a>
</div>

<div v-click class="p-4 border border-white/10 rounded">
  <div class="font-bold mb-1">Sandboxed environments</div>
  <div class="text-xs opacity-60 mb-2">Practice, not a single tool</div>
  <div class="text-sm opacity-80">Run agents in containers or with restricted permissions — Claude Code's sandbox mode, devcontainers, Docker. Let it move fast without the blast radius.</div>
</div>

</div>

<div v-click class="mt-6 text-sm opacity-60 italic text-center">
  Pick what fits your workflow — none of these are required to get started.
</div>

---
layout: center
class: text-center
---

# Thanks

<div class="text-lg opacity-70 mt-6 mb-10">
  Questions?
</div>

<div class="text-sm opacity-70 space-y-1">
  <div><a href="https://github.com/DavideSilva">github.com/DavideSilva</a></div>
  <div><a href="https://blog.davidesilva.dev/">blog.davidesilva.dev</a></div>
</div>

<div class="abs-br m-6 flex items-center gap-3 leading-none">
  <span class="text-sm opacity-50">Davide Silva</span>
  <span class="opacity-30">|</span>
  <img src="/assets/subvisual-white.svg" class="h-5 opacity-50" alt="Subvisual" />
</div>
