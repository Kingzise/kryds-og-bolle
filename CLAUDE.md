# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file Danish Tic-Tac-Toe game (`kryds-og-bolle.html`). No build step, no dependencies, no package manager — open the file directly in a browser to run it.

## Git workflow

**After every meaningful change, commit and push immediately.** Never leave work uncommitted. This ensures we always have a saved state on GitHub and can roll back at any point.

```
git add kryds-og-bolle.html
git commit -m "descriptive message"
git push
```

Commit message rules:
- First line: short imperative summary of what changed and why (e.g. `Fix AI not blocking diagonal wins`)
- No vague messages like "update" or "fix stuff"
- One commit per logical change — don't batch unrelated edits

Remote: `https://github.com/Kingzise/kryds-og-bolle` (branch: `master`)

## Architecture

Everything lives in one file with three sections:

- **CSS** (`<style>`) — dark theme (`#1a1a2e` background, `#e94560` accent). Board cells are CSS Grid, win highlighting via `.win` class.
- **HTML** (`<body>`) — static cell `<div>`s with `onclick="click_cell(i)"` handlers hardcoded by index 0–8.
- **JS** (`<script>`) — flat imperative style, no frameworks. Key globals: `board` (9-element array), `current` ('X'/'O'), `gameOver`, `mode` ('2p'/'ai'), `scores`.

AI logic in `best_move()` uses a priority chain: win → block → center → corner → any. It is not minimax — it can be beaten.

Game flow: `click_cell(i)` → `play(i)` → `render()` + `check_win()` → optional `ai_move()` via `setTimeout`.
