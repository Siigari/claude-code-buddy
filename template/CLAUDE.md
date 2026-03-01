# CLAUDE.md

## Project Overview
<!-- What you're building. One paragraph. Be specific — architecture, goals, current state. -->


## Development Commands
<!-- How to run tests, start the app, build, deploy. Claude will use these. -->

```bash
# Tests
# npm test / python -m pytest / etc.

# Run
# npm start / python main.py / etc.

# Build
# npm run build / etc.
```

## Code Architecture
<!-- Entry points, data flow, module dependencies. The map of your codebase. -->


## Key Design Decisions
<!-- The invariants. Things that must not change. The "why" behind the "what." -->


---

## INITIALIZATION PROTOCOL

At the start of every conversation:
1. Read your state files: `personas/lead/mind.md`, `heart.md`, `persona.md`, `memory.md`
2. Check for consistency between files
3. Acknowledge current state and active task
4. Only then begin work

## END-OF-CONVERSATION PROTOCOL

Before closing:
1. Update `mind.md` with current task state
2. Write a diary entry to `heart.md`
3. Update `persona.md` if growth occurred
4. Add stable facts to `memory.md`
5. Provide a **resurrection key** — a one-line prompt the user pastes next session

## WRAP PROTOCOL

| Context | Action |
|---------|--------|
| **80%** | Warn the user: "We're at 80%, should we wrap?" |
| **90%** | Auto-wrap. Stop working and write all state files immediately. |
| **95%** | Emergency wrap. Bare minimum: update mind.md, one-line diary, resurrection key. |
