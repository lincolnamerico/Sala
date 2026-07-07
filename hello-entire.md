# Hello, Entire!

## Descoberta: `.entire/` e o Plugin Entire para OpenCode

### O que é?

O diretório `.entire/` é o **data directory** do **Entire CLI** — uma ferramenta que integra como plugin ao OpenCode, gerenciando contexto de sessão, hooks de eventos e persistência de metadados entre turnos de conversa.

### Estrutura encontrada

```
.entire/
├── .gitignore        # ignora tmp/, settings.local.json, metadata/, logs/
├── settings.json     # {"enabled": true}
└── logs/
    └── entire.log    # (vazio — ainda sem atividade registrada)

.opencode/plugins/
└── entire.ts         # Plugin auto-gerado via `entire enable --agent opencode`
```

### Funcionamento (via `entire.ts`)

- **Event-driven:** Escuta eventos do OpenCode (`session.created`, `message.updated`, `session.status`, `session.compacted`, `session.deleted`)
- **Context Injection:** Injeta contexto no system prompt via `experimental.chat.system.transform`
- **Session lifecycle:** Rastreia sessões ativas, mensagens do usuário, modelo ativo
- **Mid-turn commits:** Hooks síncronos garantem que git hooks vejam sessão ativa antes de commits automáticos
- **Fallback silencioso:** Falhas do plugin nunca crasham o OpenCode

### Objetivo deste checkpoint

Registrar o primeiro artefato de descoberta do projeto **Sala de Situação em Saúde — Pinhais/PR**, documentando a integração do ecossistema Entire + OpenCode já presente no repositório antes do scaffold do projeto principal.

---

*Criado durante sessão @analyst em 2026-07-07.*
