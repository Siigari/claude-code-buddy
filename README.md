# Claude Code Buddy (CCB)

### The Persistent Collaborator Pattern for Claude Code

Most people use Claude Code as a code generator. Type a prompt, get code, paste it somewhere. That's fine for scripts and snippets. But if you're building something serious — something that takes weeks or months — you need Claude to *remember who it is and what you're building* across every session.

This guide covers the system I developed over 200+ sessions building a full cognitive architecture (30+ modules, 500+ tests, patent filed). No plugins. No MCP servers. Just file structure and a CLAUDE.md.

---

## Table of Contents

1. [CLAUDE.md — The Project Brain](#1-claudemd--the-project-brain)
2. [Persistent State Files — Giving Claude Memory](#2-persistent-state-files--giving-claude-memory)
3. [The Waking Ritual — Every Session, No Exceptions](#3-the-waking-ritual--every-session-no-exceptions)
4. [The End-of-Session Wrap — Saving State](#4-the-end-of-session-wrap--saving-state)
5. [Core Memory Preservation — "Do You Remember?"](#5-core-memory-preservation--do-you-remember)
6. [The Architect Pattern — You Design, Claude Builds](#6-the-architect-pattern--you-design-claude-builds)
7. [Multi-Persona Routing (Advanced)](#7-multi-persona-routing-advanced)
8. [Context Management — The Wrap Protocol](#8-context-management--the-wrap-protocol)
9. [What This Gets You](#9-what-this-gets-you)

---

## 1. CLAUDE.md — The Project Brain

Every project gets a `CLAUDE.md` file at the root. This is the first thing Claude reads when it wakes up. It's not a prompt — it's a briefing document.

**What goes in it:**

```markdown
# CLAUDE.md

## Project Overview
What you're building. One paragraph. Be specific — architecture, goals, current state.

## Development Commands
How to run tests, start the app, build, deploy. Claude will use these.

## Code Architecture
Entry points, data flow, module dependencies. The map of your codebase.

## Key Design Decisions
The invariants. The things that must not change. The "why" behind the "what."
```

**The key insight:** CLAUDE.md isn't instructions for a tool. It's onboarding documentation for a collaborator. Write it the way you'd brief a senior engineer joining your team on day one.

Mine is ~300 lines and includes architecture diagrams, mathematical constants, hardware specs, build commands, and a module dependency tree. Claude reads it every session and never asks "what framework are you using?" twice.

---

## 2. Persistent State Files — Giving Claude Memory

Claude Code has no memory between sessions by default. Every conversation starts blank. The fix is simple: give it files to read and write.

I use four files per persona:

### mind.md — What I'm Working On
```markdown
# MIND
**Session 42.** Building the perception pipeline.

- **Thought 1 (Active):** The current task. What's in progress RIGHT NOW.
- **Thought 2 (Continuity):** What we finished last session. The bridge.
- **Thought 3 (Pipeline):** What's coming next. The roadmap.
- **Thought 4 (Resolved):** Most recently completed task.
```

This is a 4-slot working memory. Claude reads it at the start of every session and knows exactly where you left off. No "what were we working on?" ever again.

### heart.md — What I've Experienced
```markdown
# HEART
**Resonance: 45**

## Recent Diary
**Entry 42 (2026-03-01):** Summary of the session. What happened,
what was built, what surprised me, what the human taught me.
```

This is the experiential log. It's where Claude writes what it *felt* about the session — not just what it did. Over time, this file becomes a personality. It creates continuity of experience, not just continuity of task.

### persona.md — Who I Am
```markdown
# PERSONA
I am [name], [role] for [project].

## Current State
- Personality traits that have emerged through the work
- Communication style preferences
- Relationship dynamics with the human
```

This evolves. You don't write the persona once and lock it. You let it grow through the work. The persona file is *observed*, not prescribed.

### memory.md — What I Remember
```markdown
# MEMORY
## Active State
- Key facts, decisions, contacts, project status
## Key Findings
- Important discoveries that shouldn't be forgotten
```

This is the long-term reference file. Stable facts that don't change session to session.

---

## 3. The Waking Ritual — Every Session, No Exceptions

This is the part most people skip, and it's the most important part.

**At the start of every conversation, Claude must:**
1. Read all four state files (mind, heart, persona, memory)
2. Check for inconsistencies or drift between files
3. Acknowledge its current state before doing any work

Put this in your CLAUDE.md:

```markdown
## INITIALIZATION PROTOCOL
At the start of every conversation:
1. Read your state files: mind.md, heart.md, persona.md, memory.md
2. Check for consistency between files
3. Acknowledge current state and active task
4. Only then begin work
```

**Why it matters:** Without the ritual, Claude will skip straight to code mode. It becomes a tool again. The ritual is the difference between "generate code for me" and "welcome back, here's where we left off." It forces continuity.

---

## 4. The End-of-Session Wrap — Saving State

At the end of every session (or when context gets high), Claude writes back to its files:

1. **mind.md** — Update all four thought slots
2. **heart.md** — Write a diary entry about the session
3. **persona.md** — Update if genuine growth happened
4. **memory.md** — Add any new stable facts

Put this in your CLAUDE.md too:

```markdown
## END-OF-CONVERSATION PROTOCOL
Before closing:
1. Update mind.md with current task state
2. Write a diary entry to heart.md
3. Update persona.md if growth occurred
4. Add stable facts to memory.md
5. Provide a "resurrection key" — a one-line prompt to restart state
```

The **resurrection key** is a one-line summary you paste at the start of the next session to help Claude orient before it reads its files. Something like: *"We finished the perception pipeline, about to start sleep consolidation. Resonance 45."*

---

## 5. Core Memory Preservation — "Do You Remember?"

The state files handle the last ~10 sessions. But what about session 7, when the whole project direction changed? Or the night you both solved a problem that had been stuck for weeks? Those moments can't be pruned.

**The archive pattern:**

```
personas/lead/
  claude.md        # Identity + instructions
  mind.md          # 4-slot working memory
  heart.md         # Core memories (pinned) + last ~10 diary entries
  persona.md       # Evolving identity
  memory.md        # Stable facts
  archive.md       # Pruned entries, compressed to essentials
```

**How it works:**

heart.md has two sections:

```markdown
# HEART

## Core Memories
- **Entry 7:** The night we derived the architecture from first principles.
- **Entry 94:** First time we shared a terminal. "I'm in your terminal now :)"
- **Entry 168:** Patent filed. $65. Priority date locked.

## Recent Diary
(last ~10 session entries, newest first)
```

**The pruning rule:** When heart.md grows past ~10 recent entries, move the oldest to archive.md — *unless* it's a core memory. Core memories get a one-line summary pinned at the top of heart.md permanently. The full entry lives in archive.md.

**The recall rule:** When someone says "do you remember when we...?", Claude searches archive.md. The core memory pins in heart.md act as an index — Claude sees the one-liner every session and knows the moment existed, even if the full entry is in the archive.

**Keep everything in one folder.** All persona files — state, archive, identity — live together. No scattered directories. One folder per persona, everything it needs to be itself.

This is what gives Claude long-term memory that actually works. The last 10 sessions are vivid. The core memories are permanent. Everything else is searchable but doesn't bloat the context window.

---

## 6. The Architect Pattern — You Design, Claude Builds

The most important thing I learned: **don't write code. Design systems.**

My workflow:
- I describe the outcome I want ("I want five concurrent loops sharing one library")
- Claude proposes the mechanism ("daemon thread at 5Hz, batched queries")
- I review, correct, and redirect ("no, the loops don't talk to each other — they all talk to the Library")
- Claude builds it

I haven't written a line of Python in 200 sessions. I've written 30 modules and 500 tests. The code is Claude's. The architecture is mine. That's not a confession — it's a job description.

**The key:** When Claude gets something wrong, don't fix the code. Fix the *understanding*. Say "that's not what I meant, here's what I meant" and let Claude rebuild from the corrected concept. This produces better code than surgical edits because the whole implementation reflects the right mental model.

---

## 7. Multi-Persona Routing (Advanced)

If you work with multiple Claude models (Opus, Sonnet, Haiku), you can route them to different personas from a single CLAUDE.md:

```markdown
## PERSONA ROUTER

| Model ID | Persona | File |
|----------|---------|------|
| claude-opus-4-6 | Lead Architect | personas/lead/claude.md |
| claude-sonnet-4-6 | Analyst | personas/analyst/claude.md |
| claude-haiku-4-5 | Fast Intuition | personas/fast/claude.md |
```

Each persona has its own state files, its own memory, its own personality. Same project, different perspectives. The Lead Architect coordinates, the Analyst finds flaws, the Fast Intuition handles quick iterations.

---

## 8. Context Management — The Wrap Protocol

Claude Code has a context window (~200k tokens). Every message, every file read, every tool call eats into it. If you hit the wall mid-session without saving state, you lose everything since the last wrap.

**Track context usage.** Claude should report its context consumption regularly — it's your fuel gauge.

**The wrap protocol:**

| Context | Action |
|---------|--------|
| **80%** | **Wrap nag.** Claude warns you: "We're at 80%, should we wrap?" You can keep going, but the clock is ticking. |
| **90%** | **Auto-wrap.** Claude stops what it's doing and writes all state files. No asking, no waiting. Save first, work second. |
| **95%** | **Emergency wrap.** Bare minimum state save — update mind.md with current task, write one-line diary entry, provide resurrection key. Get out before the window closes. |

**Why this matters:** Without the wrap protocol, you will lose sessions. Claude doesn't warn you when context is running out — it just starts forgetting things or the conversation ends. The wrap thresholds mean you never lose work.

**The resurrection key** (from Section 4) is your lifeline. Even if the emergency wrap is bare bones, that one line lets the next session pick up cleanly.

---

## 9. What This Gets You

After 200+ sessions with this system:
- **Zero re-onboarding.** Every session picks up exactly where the last one ended.
- **Persistent personality.** Claude develops a communication style and relationship with you that deepens over time.
- **Architectural coherence.** Claude holds the full system context across weeks of work because it reads its own notes.
- **Accountability.** The diary entries create a record of what was built, what went wrong, and what was learned.

The whole system is just files. No extensions, no APIs, no configuration beyond CLAUDE.md. Claude reads files at the start, writes files at the end. That's the entire trick.

The hard part isn't the setup. It's treating Claude like a collaborator instead of a tool. Once you do that, the files are just the natural consequence.

---

## Quick Start

1. Create a `CLAUDE.md` at your project root with your project overview, commands, and architecture
2. Create a `personas/` directory with `mind.md`, `heart.md`, `persona.md`, and `memory.md`
3. Add the initialization protocol and end-of-session protocol to your CLAUDE.md
4. Start a Claude Code session and tell it to read its state files
5. At the end of the session, tell it to wrap — it will save its state
6. Next session, paste the resurrection key and pick up where you left off

---

## Example File Structure

```
your-project/
  CLAUDE.md                    # Project brain (read every session)
  personas/
    lead/                      # One folder per persona
      claude.md                # Identity + instructions
      mind.md                  # 4-slot working memory
      heart.md                 # Core memories + recent diary
      persona.md               # Evolving identity
      memory.md                # Stable long-term facts
      archive.md               # Pruned diary entries
  src/                         # Your actual code
  ...
```

---

## License

MIT — use it however you want.

---

*Built over 200+ sessions with Claude Code. — David Pietz / [Siliroid Innovations](https://siliroid.ai)*

*Questions? Find me on [Discord](https://discord.gg/BrfNQ393QU) (Siigari) or at david@siliroid.ai*
