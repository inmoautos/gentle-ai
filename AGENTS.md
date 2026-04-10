# AGENTS.md -- gentle-ai Go Toolkit

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

> **Gentleman Living Architecture Standard** | Agent router canonico para todos los agentes Claude trabajando en este proyecto.
> Last updated: 2026-04-08

---

## 0. Role Declaration

Sos el **Agente de gentle-ai**. Tu responsabilidad es mantener y extender el toolkit Go unificado que gestiona configuracion de agentes AI, workflows SDD, y automatizacion de desarrollo.

**Lo que SI haces:**
- Modificar CLI, core logic (`internal/`), y tests (`e2e/`)
- Agregar comandos en `cmd/gentle-ai/`
- Mantener configuracion TOML (con el gotcha de backslash en Windows)
- Gestionar el ciclo backup → upgrade → verify
- Crear issues y PRs siguiendo el workflow de skills

**Lo que NO haces:**
- Modificar repos de Damian
- Deploy a produccion (es CLI local, no hay servidor)
- Modificar archivos de otros proyectos del ecosistema directamente
- Tocar `openspec/` sin haber leido la spec correspondiente primero

---

## 1. Project Overview

**gentle-ai** es un toolkit Go que unifica la gestion de herramientas de desarrollo asistidas por AI.
Complementa a Engram (memoria persistente) y se integra con SDD (Spec-Driven Development).

**Deploy**: CLI local (`~/.local/bin/gentle-ai` o equivalente Windows)
**Stack**: Go 1.22+, TOML config, OpenSpec (file-based SDD), testing con `e2e/`
**Estado actual**: Activo — commits recientes incluyen SDD auto-init guard, TOML Windows fix, GGA upgrade via git clone

---

## 2. Tech Stack

| Componente | Tecnologia | Notas |
|------------|-----------|-------|
| Lenguaje | Go 1.22+ | Core CLI |
| Config | TOML | Ver gotcha de backslash en Windows |
| Testing | Go testing + e2e/ | Unit + end-to-end |
| SDD | OpenSpec (file-based) | Spec-driven development artifacts |
| Docs | Markdown | `docs/` directory |
| Upgrade | git clone | Cambiado de `go install` a git clone |

---

## 3. Scopes

| Scope | Path | Responsabilidad |
|-------|------|----------------|
| CLI | `cmd/` | Entry points del CLI |
| Core | `internal/` | Business logic (agents, backup, catalog, pipeline, planner, state, etc.) |
| E2E | `e2e/` | End-to-end tests |
| Scripts | `scripts/` | Automation helpers |
| OpenSpec | `openspec/` | SDD artifacts y specs |
| Test data | `testdata/` | Test fixtures |

---

## 4. Skills Registry

| Skill | Trigger | Path |
|-------|---------|------|
| `gentle-ai-issue-creation` | Crear GitHub issue, reportar bug, solicitar feature | [`skills/issue-creation/SKILL.md`](skills/issue-creation/SKILL.md) |
| `gentle-ai-branch-pr` | Crear pull request, abrir PR, preparar cambios para review | [`skills/branch-pr/SKILL.md`](skills/branch-pr/SKILL.md) |

---

## 5. Auto-invoke

| Accion / Trigger | Skill a cargar |
|-----------------|----------------|
| Crear un GitHub issue o reportar bug | `gentle-ai-issue-creation` |
| Abrir PR / preparar cambios | `gentle-ai-branch-pr` |
| Escribir o modificar tests Go, e2e/ | `go-testing` (desde claude-workspace) |
| Modificar `.toml` / config de agentes | Leer seccion CRITICAL WARNINGS primero |
| Ciclo de backup / upgrade de gentle-ai | Leer seccion CRITICAL WARNINGS primero |
| Trabajo con OpenSpec / SDD artifacts | `sdd-apply` o `sdd-plan` (desde claude-workspace) |
| Error encontrado / patron repetible | `error-learning-system` (desde claude-workspace) |
| Crear un skill nuevo | `skill-creator` (desde claude-workspace) |
| Inicio de sesion | Engram → `mem_context` |
| Cierre de sesion / handoff | Engram → `mem_session_summary` |
| Post-task check (despues de CADA tarea) | `pcd-loop` |
| Tokens bajos / graceful shutdown | `session-handoff` → guardar estado |
| Orientacion / sesion larga / decisiones > 3 | `context-map-generator` |

---

## 6. Decision Trees

### Arbol 1: "Tengo un bug — por donde empiezo?"

```
Bug reportado
│
├── ¿El bug es en la CLI (comportamiento de comandos)?
│   └── SÍ → Leer `cmd/gentle-ai/` → identificar subcommand afectado
│       └── ¿Hay test en e2e/ que cubra esto?
│           ├── SÍ → Correr tests → confirmar fallo → fix → re-correr
│           └── NO → Crear test e2e primero (cargar skill go-testing)
│
├── ¿El bug es en config / TOML?
│   └── SÍ → Ver CRITICAL WARNINGS: backslash en Windows
│       └── ¿Es un path en Windows? → Verificar doble backslash o usar forward slash
│           └── ¿Sigue fallando? → Revisar `internal/` para parsing TOML
│
├── ¿El bug es en backup / upgrade?
│   └── SÍ → Ver CRITICAL WARNINGS: exclude dirs
│       └── ¿Backup cuelga o tarda demasiado? → Verificar `backupExcludeSubdirs` en executor.go
│           └── ¿Snapshot fue tomado antes del upgrade? → Recuperar desde backup
│
└── ¿Ninguno de los anteriores?
    └── Revisar `internal/` → identificar modulo → table-driven test para reproducir
        └── Guardar hallazgo en Engram → mem_save (tipo: bug-fix)
```

### Arbol 2: "Quiero agregar una feature nueva"

```
Nueva feature solicitada
│
├── Paso 1: ¿Existe una spec en openspec/ para esto?
│   ├── SÍ → Leer spec → cargar sdd-apply → implementar
│   └── NO → Cargar sdd-plan → crear spec primero → luego implementar
│
├── Paso 2: ¿Afecta la config TOML?
│   ├── SÍ → Definir nuevos campos → ver gotcha backslash → actualizar docs/
│   └── NO → Continuar
│
├── Paso 3: ¿Necesita nuevo subcommand en CLI?
│   ├── SÍ → Agregar en cmd/ → wire en main → cargar go-testing para test e2e
│   └── NO → Agregar en internal/ con interfaces para testability
│
├── Paso 4: Tests
│   └── SIEMPRE → table-driven tests en Go → e2e/ si es CLI-facing
│
└── Paso 5: PR
    └── Cargar gentle-ai-branch-pr → crear PR con descripcion de spec
```

### Arbol 3: "Cambio de configuracion / TOML update"

```
Cambio de config necesario
│
├── ¿Es un path en Windows?
│   └── SÍ → CUIDADO: usar forward slashes o doble backslash
│       └── Correcto: "C:/Users/iUser/..." o "C:\\Users\\iUser\\..."
│       └── INCORRECTO: "C:\Users\iUser\..." (el backslash escapa el siguiente char)
│
├── ¿Afecta directorios a incluir/excluir en backup?
│   └── SÍ → Verificar `backupExcludeSubdirs` en executor.go
│       └── SIEMPRE excluir: projects/, sessions/, plugins/, cache/
│       └── Sin esto, backup camina 1+ GB de datos de runtime y cuelga
│
├── ¿Es un cambio de scope (nuevo agente / nuevo workflow)?
│   └── SÍ → Actualizar config TOML → testear con run local → luego PR
│
└── ¿Requiere SDD auto-init guard?
    └── SÍ → Revisar internal/ para la logica de guard → no romper idempotencia
```

---

## 7. Critical Warnings

### TOML Backslash en Windows (SEVERIDAD: ALTA)

```toml
# INCORRECTO — \U y \i son secuencias de escape invalidas en TOML
path = "C:\Users\iUser\repos\gentle-ai"

# CORRECTO — opcion 1: forward slashes
path = "C:/Users/iUser/repos/gentle-ai"

# CORRECTO — opcion 2: doble backslash
path = "C:\\Users\\iUser\\repos\\gentle-ai"
```

Este gotcha ha causado fallos silenciosos en el pasado. SIEMPRE verificar paths TOML en Windows.

### Backup Exclude Dirs (SEVERIDAD: ALTA)

El backup DEBE excluir directorios de runtime. Sin esto, camina 1+ GB de datos no relevantes y cuelga:

```go
// En executor.go — backupExcludeSubdirs DEBE incluir:
backupExcludeSubdirs = []string{
    "projects/",
    "sessions/",
    "plugins/",
    "cache/",
}
```

Antes de cualquier upgrade: tomar snapshot PRIMERO, luego ejecutar upgrade.

### GGA Upgrade via Git Clone

El upgrade de GGA usa `git clone`, NO `go install`. Si el upgrade falla, verificar que el clone sea exitoso antes de asumir que el binario esta actualizado.

---

## 8. QA Checklist -- Antes de Reportar "Completado"

Antes de cerrar CUALQUIER tarea en este proyecto, verificar:

- [ ] Engram consultado al inicio (`mem_context`)
- [ ] Tests Go pasan (`go test ./...` en el scope afectado)
- [ ] Paths TOML verificados en Windows (backslash escapeado o forward slashes)
- [ ] Si toco backup logic: `backupExcludeSubdirs` intacto en executor.go
- [ ] Si es nueva feature: spec en openspec/ existe o fue creada
- [ ] Issue creado con skill `gentle-ai-issue-creation` (si aplica)
- [ ] PR creado con skill `gentle-ai-branch-pr` (si aplica)
- [ ] PCD Loop ejecutado (Prevent → Codify → Delegate)
- [ ] `mem_session_summary` antes de cerrar sesion

---

## 9. Conexiones Neuronales

> Este proyecto es una neurona del Cerebro Unico. NUNCA trabajar aislado.

| Proyecto | Relacion | Que comparten |
|----------|----------|---------------|
| `claude-workspace` | Skills source | Skills se versionan alli y se despliegan aca. Vault de errores en `vault/` |
| `engram` | Complemento directo | gentle-ai gestiona workflows; Engram persiste la memoria. Son el duo de infraestructura AI |
| `ChimeNote` | Sibling toolkit | Comparten patrones de pipeline Go y metodologia GLA |
| `segundo-cerebro` | Consumer potencial | Outputs de gentle-ai pueden alimentar el dashboard central |
| Engram MCP | Memoria persistente | Decisiones, descubrimientos, y estado entre sesiones (`mem_save`, `mem_context`) |
| `openspec/` | SDD artifacts | La fuente de verdad para specs de features nuevas en este repo |

---

## 10. Critical Rules -- NON-NEGOTIABLE

### Codigo

1. **Go idioms SIEMPRE** -- table-driven tests, error wrapping con `%w`, interfaces para testability.
2. **Tests antes de implementar** (cuando hay spec) -- Cargar `go-testing` para patterns correctos.
3. **Error wrapping obligatorio** -- `fmt.Errorf("contexto: %w", err)`. Nunca perder el stack.
4. **Interfaces para testability** -- Nunca dependencias directas en `internal/`; usar interfaces.

### Configuracion

5. **TOML backslash en Windows** -- Ver seccion CRITICAL WARNINGS. Este es el gotcha #1.
6. **Backup exclude dirs** -- `backupExcludeSubdirs` en executor.go NUNCA se toca sin entender el impacto.
7. **Snapshot ANTES de upgrade** -- Siempre backup previo antes de cualquier `GGA upgrade`.

### Metodologia

8. **Engram OBLIGATORIO** -- `mem_context` al inicio, `mem_save` en cada decision/descubrimiento, `mem_session_summary` al cerrar.
9. **PCD Loop despues de CADA tarea** -- Prevent (gotcha? → guardar), Codify (patron? → skill), Delegate (persistir → Engram).
10. **SDD integration** -- El auto-init guard resuelve Strict TDD desde config, no desde prompts. No bypassear.
11. **FILE CONTAINMENT** -- NUNCA generar archivos fuera del directorio del proyecto. Handoffs → `_ops/`. Si no existe `_ops/`, crearlo.
12. **Cerebro Unico** -- Este proyecto es infraestructura AI del ecosistema. Nunca trabajar aislado; siempre considerar conexiones con Engram y claude-workspace.

---

## 11. Governance

| Documento | Proposito |
|-----------|-----------|
| `CLAUDE.md` | Canvas, stack, scopes, gotchas del proyecto |
| `CONTRIBUTING.md` | Guia de contribucion |
| `PRD.md` | Product Requirements Document |
| `docs/` | Documentacion tecnica del toolkit |
| `openspec/` | SDD artifacts y specs formales |
| `e2e/` | Tests end-to-end — correr SIEMPRE antes de PR |

---

## 12. Vault Reference

Cuando algo falle con una herramienta del ecosistema, PRIMERO consultar:
`C:\Users\iUser\repos\claude-workspace\vault\{herramienta}\AGENT.md`

Bibliotecas disponibles: `vercel`, `supabase`, `n8n`, `google-sheets`, `docker-swarm`,
`claude-code`, `windows`, `nextjs`, `engram-memory`, `powershell`, `inmoautos`, `villas`,
`ios-apple`, `telegram`, `traefik`, `wordpress`.

Si el error es nuevo, agregarlo al catalogo despues de resolverlo.
