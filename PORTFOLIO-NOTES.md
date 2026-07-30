# Portfolio notes

**Category:** B — early OSS; first Obsidian plugin; took over broken fork and fixed core scroll logic

## Context

First entry into Obsidian plugin authorship. Badly maintained fork — inherited a broken, inefficient scrolling algorithm from the original author (Petr Nazarov, Nov 2022).

## Chas's first commit

**`13643c2`** — Jul 20, 2024 — *"fix this plugin: simplify scrolling algorithm"*  
https://github.com/ChasKane/autoscroll/commit/13643c2

Replaced the `currentTop` / `nextTop` state machine with a `pixelfractionCounter` accumulator. Also added mobile-friendly commands, slider settings, and later reading-mode support (`a33c7c0`).

## Original (broken) algorithm

Every 10ms tick, tracked two scroll positions and only called `scrollTo` when `nextTop - currentTop > 1`. Otherwise it incremented `nextTop` without scrolling — a two-phase state machine that was hard to reason about and behaved poorly.

## Fixed algorithm

Accumulate fractional pixels each tick; when counter ≥ 1, scroll by that amount and keep the remainder (`%= 1`). Standard sub-pixel scroll accumulation pattern.
