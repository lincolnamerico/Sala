# Project Brief: Sala de Situacao em Saude - Pinhais

## Executive Summary

Sistema de dashboard georreferenciado para a Sala de Situacao em Saude de Pinhais/PR, integrando indicadores do financiamento federal da Atencao Primaria (Programa Mais Saude da Familia - Portaria GM/MS N° 3493/2024) com dados epidemiologicos municipais. A solucao substitui planilhas manuais e relatorios estaticos por um painel interativo que permite ao gestor municipal monitorar metas de financiamento, identificar desvios em tempo real e direcionar acoes por UBS.

## Problem Statement

- **Estado atual:** Pinhais nao possui sistema integrado para monitorar indicadores de saude. Dados do e-SUS APS, SIA/SIH, SINAN e demais fontes federais sao extraidos manualmente e consolidados em planilhas, com defasagem de semanas.
- **Impacto:** Risco de perda de repasse financeiro federal por nao acompanhamento tempestivo das metas do Programa Mais Saude da Familia. Decisoes baseadas em dados defasados ou inexistentes. Impossibilidade de estratificacao por territorio/UBS.
- **Urgencia:** A nova politica instituida pela Portaria GM/MS N° 3493/2024 alterou a logica de repasses, focando novamente na Estrategia Saude da Familia (ESF) com enfase no cuidado integral e longitudinalidade. Exige monitoramento continuo de indicadores de producao, cobertura e qualidade vinculados a ESF para garantia do repasse integral.

## Proposed Solution

Dashboard web responsivo com 3 camadas:
1. **Camada Operacional (Gestor):** Indicadores do financiamento federal por UBS com meta vs realizado, comparativo mensal e anual
2. **Camada Analitica (Epidemiologista):** Serie historica com canal endemico, alertas por desvio sazonal, correlacao financiamento-epidemiologia (fase posterior)
3. **Camada Cidada (Comunidade):** Alerta simples por microarea com recomendacoes praticas (fase posterior)

Abordagem hibrida (Caminho C): API hibrida + indicadores combo + 4 niveis de estratificacao com heatmap geografico.

## Target Users

### Gestor Municipal de Saude (Sr. Almeida)
- **Perfil:** Gestor publico, tomador de decisao, sem habilidades tecnicas avancadas
- **Necessidades:** Visao mensal por UBS, alertas inteligentes (so quando necessario), comparativo com mesmo periodo anterior, monitoramento do financiamento federal
- **User Stories:** US-GES-01 a US-GES-04

### Epidemiologista (Dra. Carla)
- **Perfil:** Profissional de saude publica, analisa tendencias e surtos
- **Necessidades:** Serie historica com media movel, canal endemico por SE, dupla hierarquia geografica (bairro e UBS)
- **User Stories:** US-ECO-01 a US-ECO-04

### Cientista de Dados (Leo)
- **Perfil:** Tecnico responsavel pelo pipeline e modelo de dados
- **Necessidades:** ETL dos 5 sistemas fonte, dupla hierarquia com crosswalk versionada, algoritmo de alerta por z-score sazonal
- **User Stories:** US-DS-01 a US-DS-04

### Cidadao (Dona Jurema)
- **Perfil:** Morador de Pinhais, busca informacao clara e acionavel
- **Necessidades:** Alerta por microarea do ACS, linguagem simples, recomendacoes praticas
- **User Stories:** US-COM-01 a US-COM-03

## Goals & Success Metrics

### Business Objectives
- Garantir repasse integral do financiamento federal da APS
- Reduzir tempo entre coleta de dado e tomada de decisao de semanas para dias
- Substituir planilhas manuais por dashboard automatizado

### User Success Metrics
- Gestor consegue identificar UBS abaixo da meta em < 5 minutos
- Epidemiologista valida alerta de surto com 1 clique no canal endemico
- Cidadao entende o status de saude do seu bairro em 30 segundos

### Key Performance Indicators (KPIs)
- Taxa de execucao de visitas domiciliares (% realizado vs programado)
- Proporcao de gestantes com 7+ consultas de pre-natal
- Cobertura de hipertensos cadastrados

## MVP Scope

### Core Features (Must Have)
- **Pipeline e-SUS APS** para PostgreSQL com carga historica de 12 meses
- **Hierarquia Sanitaria** (Municipio > UBS > Microarea ACS) com crosswalk simples
- **Dashboard Gestor** com 2 indicadores do Programa Mais Saude da Familia (hipertensos + gestantes) + visitas domiciliares, agregado por UBS, meta vs realizado, comparativo mensal
- **Alerta por Limiar Fixo** quando indicador cai abaixo da meta

### Out of Scope for MVP
- Hierarquia Territorial (Bairro > Setor Censitario)
- Drill-across entre hierarquias geograficas
- Classificacao Donabedian exposta no dashboard
- Algoritmo de alerta com z-score + delay correction
- Painel Cidadao (Dona Jurema)
- Integracao SINAN, SIA/SIH, SIM, CNES
- Correlacao financiamento x epidemiologia
- PostGIS com geometrias complexas

### MVP Success Criteria
- Gestor visualiza dashboard funcional com dados reais de Pinhais
- Os 3 indicadores carregam corretamente por UBS
- Comparativo com mes anterior e mesmo mes do ano anterior funciona
- Alerta dispara quando indicador esta abaixo da meta

## Post-MVP Vision

### Phase 2 Features
- PostGIS + geometria das UBS + cruzamento com bairros via crosswalk versionada
- Algoritmo de alerta inteligente (z-score + fator de correcao de atraso)
- Integracao SINAN e SIH (dengue, ICSAP)
- Painel epidemiologico com canal endemico

### Phase 3 Features
- Hierarquia territorial completa (bairro > setor censitario)
- Painel Cidadao com alerta por microarea e recomendacoes
- Correlacao financiamento x epidemiologia por UBS

## Technical Considerations

### Platform Requirements
- **Target Platforms:** Web responsivo (desktop + mobile)
- **Browser Support:** Chrome, Firefox, Edge - versoes recentes
- **Performance:** Dashboard carrega em < 3s, queries geo < 500ms

### Technology Preferences
- **Frontend:** Next.js + React + TypeScript + Tailwind CSS
- **Backend:** Next.js API routes + Node.js
- **Database:** PostgreSQL + PostGIS (fase 2)
- **Hosting:** Cloudflare ou Vercel (a definir)

### Architecture Considerations
- **Repository:** Monorepo no padrao AIOX
- **Pipeline:** ETL com orquestrador (Airflow ou script cron + load incremental)
- **Crosswalk:** Tabela auxiliar `ubs_bairro` com `valid_from`/`valid_to` sem PostGIS no MVP

## Constraints & Assumptions

### Constraints
- **Timeline:** Sem prazo definido, evolucao incremental
- **Resources:** Equipe pequena, desenvolvimento em paralelo com outras demandas
- **Technical:** Depende de acesso aos sistemas fonte do Ministerio da Saude (e-SUS APS, SIA/SIH etc.)

### Key Assumptions
- SMS Pinhais validara as 15 user stories antes do inicio do desenvolvimento
- O e-SUS APS esta disponivel e exportavel em formato processavel (CSV/API)
- A crosswalk UBS x bairro pode ser definida em reuniao com a SMS (Sprint 0)
- O Gestor e o principal stakeholder e participara de reviews semanais

## Risks & Open Questions

### Key Risks
- **Crosswalk sem versionamento:** Dados historicos perdem consistencia se areas mudarem
- **Volatilidade do financiamento federal:** Regras do Programa Mais Saude da Familia podem mudar por novas portarias ministeriais
- **Subnotificacao em dados diarios:** Feriados e finais de semana geram ruido
- **Baixa aderencia do gestor:** Dashboard poderoso mas complexo nao sera usado
- **Escopo alem da capacidade:** Projeto grande para equipe pequena

### Open Questions
- Qual orquestrador de ETL utilizar? (Airflow vs cron + script)
- PostGIS e necessario no MVP ou tabela auxiliar resolve?
- O gestor prefere notificacao por email, SMS ou push?
- Quem fara a manutencao do sistema apos entrega?

### Areas Needing Further Research
- Formato real dos dados do e-SUS APS exportados por Pinhais
- Quantidade de UBS ativas e areas de abrangencia atualizadas
- Regras vigentes do Programa Mais Saude da Familia (Portaria GM/MS N° 3493/2024) e eventuais atualizacoes

## Appendices

### A. Contexto Normativo

A Portaria GM/MS N° 3493/2024 instituiu o **Programa Mais Saude da Familia**, substituindo o anterior Previne Brasil. As principais alteracoes:

- **Retorno ao foco na ESF:** O financiamento volta a priorizar a Estrategia Saude da Familia, valorizando equipes completas (medico, enfermeiro, dentista, ACS) e nao apenas indicadores individuais
- **Cuidado integral e longitudinalidade:** Os indicadores passam a considerar o acompanhamento continuo do paciente ao longo do tempo, nao apenas procedimentos isolados
- **Logica de repasses:** Alteracao na ponderacao dos componentes (capitacao ponderada, pagamento por desempenho e incentivo para acoes estrategicas)
- **Novos indicadores:** Alguns indicadores do Previne Brasil foram mantidos, outros substituidos ou reponderados — necessidade de revisao com a SMS Pinhais para confirmar quais estao vigentes

Esta mudanca reforca a importancia de um sistema flexivel, com indicadores parametrizaveis externamente (YAML/DB table) para absorver futuras alteracoes normativas sem necessidade de deploy.

### B. Elicitation Summary
8 rodadas de Advanced Elicitation realizadas via Atlas (@analyst):
1. Stakeholder Roundtable (Indicadores diarios vs sazonais)
2. User Story Qualification (financiamento APS, dupla hierarquia)
3. Critique and Refine (inconsistencias e gaps)
4. Identify Potential Risks (6 riscos mapeados)
5. Analyze Logical Flow and Dependencies (mapa de dependencias e fases)
6. Agile Team Perspective Shift (PO, SM, Dev, QA)
7. Escape Room Challenge (B + 1 + vii)
8. Red Team vs Blue Team (PostGIS vs tabela auxiliar)
9. Challenge from Critical Perspective (YAGNI — corte de escopo)

### C. Stakeholder Input
Decisoes consolidadas:
- Caminho C (Hibrido) nas 3 arvores do Tree of Thoughts
- Fluxo AIOX nativo (sem tlc-spec-driven)
- Mode: Interactive (pre-flight nao necessario)
- MVP: e-SUS APS + 2 indicadores do Programa Mais Saude da Familia + visitas domiciliares
- Corte YAGNI: hierarquia territorial, Donabedian exposto, 5 fontes

## Next Steps

1. Validar user stories com SMS Pinhais (Gestor + ACS)
2. Realizar Sprint 0 com SMS para definir crosswalk UBS x areas
3. Handoff para @architect (detalhamento arquitetonico)
4. Handoff para @dev (implementacao do MVP)
5. Configurar pipeline e-SUS APS > PostgreSQL
6. Construir dashboard Gestor com os 3 indicadores
7. Agendar review semanal com Gestor
