# Sala de Situação em Saúde — Pinhais

Dashboard georreferenciado para monitoramento de indicadores da Atenção Primária à Saúde (APS) do município de Pinhais/PR, integrando dados do financiamento federal (Programa Mais Saúde da Família — Portaria GM/MS N° 3493/2024) com epidemiologia municipal.

## Problema

Pinhais não possui sistema integrado para monitorar todos os seus indicadores de saúde. Dados do e-SUS APS, SIA/SIH e demais fontes federais e estaduais são consumidos diretamente nas respectivas fontes e/ou consolidados localmente em planilhas com defasagem de semanas. O Sistema de Prontuário Eletrônico utilizado exporta a produção ao MS mas o uso dos Painéis de BI oferecidos aos gestores, possui baixa adesão. Isso gera:

- Não acompanhamento tempestivo das metas
- Decisões baseadas em dados defasados ou inexistentes
- Estratificação por território/UBS/microárea ainda carece de aperfeiçoamentos

## Solução

Dashboard web responsivo com 3 camadas evolutivas:

1. **Camada Operacional (Gestor)** — Indicadores do financiamento federal por UBS com meta vs. realizado, comparativo mensal e anual
2. **Camada Analítica (Epidemiologista)** — Série histórica com canal endêmico, alertas por desvio sazonal (fase posterior)
3. **Camada Cidadã (Comunidade)** — Alerta simples por microárea com recomendações práticas (fase posterior)

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Frontend | Next.js 15 + React 19 + TypeScript |
| Estilização | Tailwind CSS 3 |
| Formulários | React Hook Form + Zod |
| Estado/Server | TanStack React Query + Zustand |
| Database | PostgreSQL (PostGIS em fase 2) |
| Testes | Vitest + Testing Library + Playwright |
| ORM | Prisma (schema pendente) |
| Qualidade | ESLint + Husky + TypeScript strict |

## Arquitetura

- **Monorepo** no padrão AIOX com sincronização multi-IDE (Claude Code, Cursor, Codex CLI, Gemini, GitHub Copilot, Kimi, Antigravity)
- **Pipeline ETL:** e-SUS APS → PostgreSQL
- **Hierarquia geográfica MVP:** Sanitária (Município > UBS > Microárea ACS)
- **Crosswalk:** Tabela auxiliar `ubs_bairro` com `valid_from/valid_to` (PostGIS postergado para fase 2)
- **Alerta:** Limiar fixo no MVP (z-score sazonal em fase posterior)

## MVP

**3 indicadores:** cobertura de hipertensos, proporção de gestantes com 7+ consultas, visitas domiciliares

**Entregas:**
- Pipeline e-SUS APS → PostgreSQL com carga histórica de 12 meses
- Dashboard Gestor com meta vs. realizado por UBS, comparativo mensal/anual
- Alerta por limiar fixo quando indicador cai abaixo da meta

## Estrutura

```
sala/
├── .aiox-core/          # Framework AIOX (runtime, agents, tasks, workflows)
├── .claude/             # Claude Code config + agents
├── .cursor/             # Cursor rules + agents
├── .codex/              # Codex CLI skills + agents
├── .github/             # GitHub Copilot + agent definitions
├── .gemini/             # Gemini config + commands
├── .kimi/               # Kimi skills
├── .antigravity/        # Antigravity agents
├── .husky/              # Git hooks
├── .specify/            # Specify design tokens
├── .next/               # Next.js build output
├── docs/                # Documentação do projeto
├── package.json
├── AGENTS.md            # Instruções para agentes de IA
└── README.md
```

> `src/`, `packages/`, `tests/`, `squads/` serão criados no scaffolding da aplicação.

## Desenvolvimento

```bash
# Instalar dependências
npm install

# Servidor de desenvolvimento
npm run dev

# Build de produção
npm run build

# Testes
npm test

# Lint
npm run lint

# TypeScript check
npm run typecheck

# Sincronizar definições entre IDEs
npm run sync:ide

# Validar estrutura
npm run validate:structure
```

## Scripts Disponíveis

| Comando | Descrição |
|---------|-----------|
| `npm run dev` | Next.js dev server |
| `npm run build` | Next.js build |
| `npm run lint` | ESLint (flat config) |
| `npm run typecheck` | `tsc --noEmit` |
| `npm test` | Vitest run |
| `npm run sync:ide` | Sincroniza agentes entre IDEs |
| `npm run validate:structure` | Valida estrutura de diretórios |
| `npm run validate:agents` | Valida definições de agentes |

## Equipe de Agentes AIOX

| Agente | Codinome | Função |
|--------|----------|--------|
| @pm | Morgan | Product Manager |
| @po | Pax | Product Owner |
| @sm | River | Scrum Master |
| @architect | Aria | Arquiteto |
| @dev | Dex | Desenvolvedor |
| @qa | Quinn | QA / Testes |
| @analyst | Atlas | Business Analyst |
| @devops | Gage | DevOps / Git |
| @data-engineer | Dara | Database |
| @ux-design-expert | Uma | UX/UI Design |

## Status do Projeto

**Fase:** Discovery (pré-desenvolvimento) — brief e user stories consolidados.

**Próximos passos:**
1. Validar user stories com SMS Pinhais (Sprint 0)
2. Definir crosswalk UBS x áreas geográficas
3. Handoff para arquitetura técnica detalhada
4. Implementação do MVP

---

*Projeto integrado ao [Synkra AIOX](https://github.com/lincolnamerico/Sala) — meta-framework de orquestração de agentes.*
