# AGENTS.md — Sala (Synkra AIOX)

Instrucoes para agentes OpenCode/Codex CLI neste repositorio.

## Projeto

Dashboard de saude publica para Pinhais/PR (Next.js 15 + React 19 + TypeScript + Tailwind 3), construido sobre o meta-framework Synkra AIOX.

**Status:** Pre-desenvolvimento (Discovery). `src/`, `packages/`, `tests/`, `squads/` ainda nao existem. `docs/brief.md` existe.

## Layers (Nao Modificar / Trabalhar)

| Layer | Paths | Regra |
|-------|-------|-------|
| L1-L2 (framework) | `.aiox-core/core/`, `.aiox-core/development/tasks/`, `.aiox-core/development/workflows/`, `.aiox-core/infrastructure/`, `.aiox-core/constitution.md` | NAO modificar (deny rules em `.claude/settings.json`) |
| L3 (config) | `.aiox-core/data/`, `core-config.yaml` | Mutavel |
| **L4 (projeto)** | `docs/stories/`, `packages/`, `squads/`, `tests/` | **Trabalho do projeto** |

## Comandos

| Comando | Descricao |
|---------|-----------|
| `npm run dev` | Next.js dev server |
| `npm run build` | Next.js build |
| `npm run lint` | ESLint (flat config) |
| `npm run typecheck` | `tsc --noEmit` |
| `npm test` | Vitest run |
| `npm run sync:ide` | Sincroniza agentes entre IDEs |
| `npm run validate:structure` | Valida estrutura |
| `npm run validate:agents` | Valida definicoes de agentes |

**Quality gates (ordem!):** `lint -> typecheck -> test -> build`

## Convencoes

- **CLI First:** Toda funcionalidade nova funciona 100% via CLI antes de qualquer UI
- **Story-Driven:** Nenhum codigo sem story em `docs/stories/`
- **Commits:** Convencionais (`feat:`, `fix:`, `chore:`) com story ID
- **Apenas @devops** faz `git push`, PR, release/tag (hook enforce `.claude/settings.local.json`)
- **Husky:** `npm test` roda em pre-commit e pre-push
- **Absolute imports:** Preferir `@/` sobre `../../../`
- **Multi-IDE:** Agentes em `.aiox-core/development/agents/` (fonte unica), sincronizados via `npm run sync:ide`

## Ativacao de Agentes (OpenCode)

Agentes definidos em `.opencode/opencode.jsonc`, skills em `.claude/skills/`. Atalhos:

- `@architect`, `@dev`, `@qa`, `@pm`, `@po`, `@sm`, `@analyst`, `@devops`, `@data-engineer`, `@ux-design-expert`, `@squad-creator`, `@aiox-master`

Cada agente carrega definicao de `.aiox-core/development/agents/` e skill correspondente.

<!-- AIOX-MANAGED-START: core -->
## Core Rules

1. Siga a Constitution em `.aiox-core/constitution.md`
2. Priorize `CLI First -> Observability Second -> UI Third`
3. Trabalhe por stories em `docs/stories/`
4. Nao invente requisitos fora dos artefatos existentes
<!-- AIOX-MANAGED-END: core -->

<!-- AIOX-MANAGED-START: quality -->
## Quality Gates

- Rode `npm run lint`
- Rode `npm run typecheck`
- Rode `npm test`
- Atualize checklist e file list da story antes de concluir
<!-- AIOX-MANAGED-END: quality -->

<!-- AIOX-MANAGED-START: codebase -->
## Project Map

- Core framework: `.aiox-core/`
- CLI entrypoints: `bin/`
- Shared packages: `packages/`
- Tests: `tests/`
- Docs: `docs/`
<!-- AIOX-MANAGED-END: codebase -->

<!-- AIOX-MANAGED-START: commands -->
## Common Commands

- `npm run sync:ide`
- `npm run sync:ide:check`
- `npm run sync:skills:codex`
- `npm run sync:skills:codex:global` (opcional; neste repo o padrao e local-first)
- `npm run validate:structure`
- `npm run validate:agents`
<!-- AIOX-MANAGED-END: commands -->

<!-- AIOX-MANAGED-START: shortcuts -->
## Agent Shortcuts

Preferencia de ativacao no Codex CLI:
1. Use `/skills` e selecione `aiox-<agent-id>` vindo de `.codex/skills` (ex.: `aiox-architect`)
2. Se preferir, use os atalhos abaixo (`@architect`, `/architect`, etc.)

Interprete os atalhos abaixo carregando o arquivo correspondente em `.aiox-core/development/agents/` (fallback: `.codex/agents/`), renderize o greeting via `generate-greeting.js` e assuma a persona ate `*exit`:

- `@architect`, `/architect`, `/architect.md` -> `.aiox-core/development/agents/architect.md`
- `@dev`, `/dev`, `/dev.md` -> `.aiox-core/development/agents/dev.md`
- `@qa`, `/qa`, `/qa.md` -> `.aiox-core/development/agents/qa.md`
- `@pm`, `/pm`, `/pm.md` -> `.aiox-core/development/agents/pm.md`
- `@po`, `/po`, `/po.md` -> `.aiox-core/development/agents/po.md`
- `@sm`, `/sm`, `/sm.md` -> `.aiox-core/development/agents/sm.md`
- `@analyst`, `/analyst`, `/analyst.md` -> `.aiox-core/development/agents/analyst.md`
- `@devops`, `/devops`, `/devops.md` -> `.aiox-core/development/agents/devops.md`
- `@data-engineer`, `/data-engineer`, `/data-engineer.md` -> `.aiox-core/development/agents/data-engineer.md`
- `@ux-design-expert`, `/ux-design-expert`, `/ux-design-expert.md` -> `.aiox-core/development/agents/ux-design-expert.md`
- `@squad-creator`, `/squad-creator`, `/squad-creator.md` -> `.aiox-core/development/agents/squad-creator.md`
- `@aiox-master`, `/aiox-master`, `/aiox-master.md` -> `.aiox-core/development/agents/aiox-master.md`
<!-- AIOX-MANAGED-END: shortcuts -->
