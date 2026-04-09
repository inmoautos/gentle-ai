# Gentle AI — Go Toolkit for AI-Assisted Development

<!-- gla:seguir-regla-trigger -->
## Universal Trigger: "seguir regla" (fuzzy, typos tolerados)

> Directiva Allan Guerrero 2026-04-09: este trigger DEBE respetarse en este agente, sin excepcion.

**Variantes reconocidas** (activan el protocolo GLA completo):
- "seguir regla", "seguir reglas", "sigue regla", "sigue la regla"
- "siguir regla" (typo), "seguire regla" (typo), "seguime la regla"
- "sigueregla", "seguirregla" (sin espacio)
- "follow rule", "apply rules", "activar regla"

**Regla de interpretacion fuzzy**: si el texto del usuario contiene `seguir/sigue/siguir/seguire` cerca de `regla/regia/rela`, activar el protocolo completo.

**Protocolo que se ejecuta** (ver `skills/seguir-regla/SKILL.md` para detalles):
1. Cargar canonical (`_ops/GLA_CANONICAL_RULES_2026-04-09.md`)
2. Leer CLAUDE.md global + hub + proyecto actual
3. Leer BATON.md + ROADMAP.md
4. Cargar Engram context + searches criticos
5. Verificar los 5+ mecanismos de enforcement
6. Lanzar 3 jueces HEALTH en paralelo
7. Consolidar veredictos
8. Reportar estado al usuario

**Skill canonical**: `C:/Users/iUser/repos/claude-workspace/skills/seguir-regla/SKILL.md`

**IMPORTANTE**: este trigger NO es opcional. Ignorarlo = violacion directa de la directiva GLA.
<!-- /gla:seguir-regla-trigger -->

<!-- gla:pending-audit-2026-04-09 -->
> PENDING AUDIT — PRIMER ACTO OBLIGATORIO DE ESTE CHAT (antes de responder cualquier otra cosa)
>
> Allan Guerrero (2026-04-09) ordeno que el siguiente primer chat audite TODO el trabajo de Session 9b con MCP Context7 ANTES de cualquier otra accion.
>
> Pasos: (1) ToolSearch context7 -> si no carga, reportar a Allan. (2) Leer C:/Users/iUser/repos/claude-workspace/_ops/HANDOFF_2026-04-09_SESSION9B_CANONICAL.md seccion ACCION #0. (3) Ejecutar las 10 queries context7. (4) Escribir reporte a C:/Users/iUser/repos/claude-workspace/_ops/AUDIT_CONTEXT7_SESSION9B_POST_<fecha>.md. (5) Guardar neurona audit/context7-post-session-9b. (6) Eliminar este bloque cuando este completo.
>
> Si ya se completo: verificar con mem_search "audit context7 post session 9b" antes de re-ejecutar.
<!-- /gla:pending-audit-2026-04-09 -->
> Gentleman Living Architecture | Canvas: inline below

## Methodology: Gentleman Living Architecture
Workflow: Canvas → Orient → Plan → Delegate → Synthesize → Judge → PCD Loop → Hand Off

## Canvas
- **Problema**: AI-assisted development tools are scattered — each needs its own setup, config, and conventions.
- **Usuario**: Allan Guerrero + developers using Claude Code and similar AI coding tools.
- **Solución**: Unified Go toolkit that manages AI agent configuration, SDD workflow, and development automation.
- **Fuera de scope**: Not an AI model, not a chat interface. It's a TOOLCHAIN — manages the workflow around AI tools.

## Rules

> **RULE #-1 — GLA CANONICAL MANDATORY**: before ANY action, read `C:\Users\iUser\repos\claude-workspace\_ops\GLA_CANONICAL_RULES_2026-04-09.md`. It is the SINGLE source of truth for the ecosystem. Rules below are this repo's operational subset.

### Inherited Universal Rules (U1-U18 from GLA Canonical)

> These 18 rules apply to ALL ecosystem repos. They are NOT optional. See canonical for evidence and context.

- **U-1 Read BATON first** — read `_ops/BATON.md` before any action
- **U-2 Conventional commits** — `feat:`, `fix:`, `chore:`, `docs:`; never "Co-Authored-By", never AI attribution
- **U-3 Credentials never in code** — always env vars; `.env`/`.env.local`/`credentials.json` in `.gitignore`
- **U-4 Canonical PCD Loop** — Prevent (neurona) / Codify (skill) / Delegate (Engram + BATON). SACRED, do not rename the acronym
- **U-5 Engram mandatory** — `mem_context` at start, `mem_save` per decision, `mem_session_summary` at close
- **U-6 Damián = UNTOUCHABLE** — repos, `[DT]` workflows, Gentleman upstream read-only
- **U-7 Verify before reporting** — curl/API/screenshot; "I think it works" does not count
- **U-8 Graceful shutdown** — <15% context → stop, document, push, never half-done prod
- **U-9 File containment** — nothing outside the repo; handoffs → `_ops/`, credentials gitignored
- **U-10 Single brain** — consult hub, Engram, related projects before acting
- **U-11 RAM sanity** — every 30 min: pause, save to Engram, close non-essential terminals
- **U-12 Context7 MCP mandatory** — before technical claims; "I think it's this way" is an excuse
- **U-13 Sub-agents write to disk** — `Agent()` deliverable to absolute path; output files evaporate on compaction
- **U-14 Deep audit pre-execution** — 6 mandatory steps: mem_search, vault, SKILL_INDEX, _ops, git log, Context7
- **U-15 Leverage-first** — skills/MCP/vault/Engram before own knowledge
- **U-16 5-step Error Learning** — mem_save + error-catalog + solution-catalog + skill + preventive hook
- **U-17 STOP on question** — you ask → you stop and wait; DO NOT continue in the same message
- **U-18 Guatemalan Spanish** — internal docs; English only for public libs on GitHub. **U-18 exception**: public GitHub project, English OK — DO NOT mix languages within same file.

### Project-Specific Rules

1. **Go idioms**: table-driven tests, error wrapping, interfaces for testability
2. **SDD integration**: auto-init guard resolves Strict TDD from config, not prompts
3. **TOML config**: escape backslashes on Windows (known gotcha)
4. **PCD Loop (mandatory)**: After every task → Prevent (neurona?), Codify (skill?), Delegate (engram saved?)

## Stack
| Component | Technology | Purpose |
|-----------|-----------|---------|
| Language | Go 1.22+ | Core CLI |
| Config | TOML | Agent configuration |
| Testing | Go testing + e2e/ | Unit + end-to-end |
| SDD | OpenSpec (file-based) | Spec-driven development artifacts |
| Docs | Markdown | docs/ directory |

## Scopes
| Scope | Path | Responsibility |
|-------|------|----------------|
| CLI | `cmd/` | Entry points |
| Core | `internal/` | Business logic |
| E2E | `e2e/` | End-to-end tests |
| Scripts | `scripts/` | Automation helpers |
| OpenSpec | `openspec/` | SDD artifacts |
| Test data | `testdata/` | Test fixtures |

## Skills (2 project skills)
See `AGENTS.md` for skill routing (issue-creation, branch-pr).

## Current State
- **Health**: Active, recent commits: SDD auto-init guard, TOML Windows fix, GGA upgrade via git clone
- **Deploy**: Local CLI tool
- **Tests**: e2e/ directory exists with test infrastructure
- **License**: MIT

## Gotchas
| Gotcha | Severity | Notes |
|--------|----------|-------|
| TOML backslash on Windows | HIGH | Must escape, known fix in recent commits |
| GGA upgrade uses git clone | MEDIUM | Changed from go install to git clone |
| Backup skip directories | HIGH | Pre-upgrade snapshot MUST exclude runtime dirs (projects/, sessions/, plugins/, cache/) via `backupExcludeSubdirs` in executor.go — without this, backup walks 1+ GB of non-config data and hangs |

## Vault Reference (Biblioteca de Conocimiento)

Cuando algo falle con una herramienta del ecosistema, PRIMERO consultar:
`C:\Users\iUser\repos\claude-workspace\vault\{herramienta}\AGENT.md`

Bibliotecas disponibles: vercel, supabase, n8n, google-sheets, docker-swarm,
claude-code, windows, nextjs, engram-memory, powershell, inmoautos, villas,
ios-apple, telegram, traefik, wordpress.

Si el error es nuevo, agregarlo al catalogo despues de resolverlo.


