# Roadmap — gentle-ai

> **Last updated**: 2026-04-09 (Session 9 — initial creation by RoadmapBuilder)
> **Source of truth**: this file. Everything else is derived.
> **Language exception**: U-18 does not apply — public GitHub project, English OK

## 1. Vision in one sentence

Go toolkit for AI-driven development. SDD integration, auto-init guard, TOML config. Part of Gentleman Programming.

## 2. Current status

**Status**: MVP
**Since**: 2025-12
**Reason**: Tooling for developers using AI agents with Strict TDD.

## 3. Completed milestones

- [x] Go idioms + table-driven tests
- [x] SDD integration via auto-init guard
- [x] TOML config (Windows backslash escape gotcha documented)
- [x] Session 9: applied GLA Canonical U1-U18 (English exception)

## 4. In progress

- [ ] **Plugin API expansion** (backlog)
  - Support for more AI providers

## 5. Upcoming milestones (P0/P1/P2)

### P0 (critical)
- (none right now)

### P1 (important)
- [ ] Integration tests with engram
- [ ] Better Windows path handling

### P2 (nice to have)
- [ ] Bubbletea TUI for config management

## 6. Out of scope

- NO break SDD workflow compatibility

## 7. Cross-project dependencies

| Project | What this provides | What the other provides | Status |
|---------|--------------------|--------------------------|--------|
| engram | Memory backend | Persistent context | ✓ |
| claude-workspace | Consumer + skill provider | GLA methodology | ✓ |

## 8. Last update

- **Date**: 2026-04-09
- **Session**: claude-workspace Session 9
- **Chat**: RoadmapBuilder (manual generation via script after agent crashed with 529 overloaded)
- **Changes**: Initial roadmap creation based on CLAUDE.md + BATON.md + git log + Session 9 GLA Canonical propagation
