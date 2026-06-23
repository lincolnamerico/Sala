# AGENTS.md - Synkra AIOX (OpenCode / Codex CLI)

Arquivo de instrucoes para agentes trabalhando neste repositorio.

## Contexto do Repositorio

- **Projeto:** Synkra AIOX (meta-framework de orquestracao de agentes), codename "Sala"
- **Stack:** Next.js 15 + React 19 + TypeScript + Tailwind CSS + Prisma (schema pendente)
- **Status:** Projeto em fase de scaffolding — `src/`, `docs/`, `packages/`, `tests/`, `squads/` ainda nao existem
- **Framework runtime:** `.aiox-core/` — NAO modificar (protegido por deny rules, L1-L2)
- **Trabalho do projeto (L4):** `docs/stories/`, `packages/`, `squads/`, `tests/`

## Comandos Essenciais

| Comando | Descricao |
|---------|-----------|
| `npm run dev` | Next.js dev server |
| `npm run build` | Next.js build |
| `npm run lint` | ESLint (flat config) |
| `npm run typecheck` | `tsc --noEmit` |
| `npm test` | Vitest run |
| `npm run sync:ide` | Sincroniza definicoes de agentes entre IDEs |
| `npm run validate:structure` | Valida estrutura de diretorios |
| `npm run validate:agents` | Valida definicoes de agentes |

**Ordem de verificacao:** `lint -> typecheck -> test -> build` (Quality Gates, Constitution Art. V)

## Arquitetura

- **Multi-IDE:** Claude Code, Cursor, Codex CLI, Gemini, GitHub Copilot, Kimi, Antigravity — todos sincronizados via `npm run sync:ide`
- **Agentes locais em:** `.aiox-core/development/agents/` (fonte unica), sincronizados para `.codex/agents/`, `.claude/commands/AIOX/agents/`, `.github/agents/`, `.cursor/rules/agents/`, etc.
- **Framework (L1-L2, NAO modificar):** `.aiox-core/core/`, `.aiox-core/development/tasks/`, `.aiox-core/development/workflows/`, `.aiox-core/infrastructure/`, `.aiox-core/constitution.md`
- **Project Config (L3, mutavel):** `.aiox-core/data/`, `core-config.yaml`
- **Deny rules** em `.claude/settings.json` bloqueiam edicao de L1-L2

## Convencoes

- **CLI First:** Toda funcionalidade nova DEVE funcionar 100% via CLI antes de qualquer UI (Constitution Art. I)
- **Story-Driven:** Nenhum codigo sem story associada em `docs/stories/` (Constitution Art. III)
- **Absolute Imports:** Preferir `@/` sobre `../../../` (Constitution Art. VI)
- **Commits:** Convencionais (`feat:`, `fix:`, `chore:`, etc.) com referencia a story ID
- **Apenas @devops** faz `git push`, PR, release/tag (Constitution Art. II)
- **CodeRabbit:** Review automatizado via WSL (Ubuntu), severities CRITICAL/HIGH auto-fix

## Peculiaridades Tecnicas

- **Husky:** `npm test` roda em pre-commit e pre-push
- **MCP Gateway:** Docker-based em `localhost:8080`, preset `minimal` (default) ou `full`
- **Prisma:** Em devDependencies mas sem schema ainda (`prisma/schema.prisma` nao existe)
- **Configs ausentes:** `next.config.*`, `tailwind.config.*`, `vitest.config.*`, `eslint.config.*` — serao criados conforme necessario
- **Nao ha CI workflows** em `.github/workflows/` ainda
- **Navegacao:** Usar Grep/Glob (nao `grep`/`rg` no bash)

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
