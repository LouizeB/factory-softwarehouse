# factory-softwarehouse

Motor de produção da software house de design systems: pipeline de 7 fases operado por agentes, com gates humanos obrigatórios. Este repositório contém as **skills** (uma por fase) e os **prompts de agente** — é o ativo da agência e não é entregue ao cliente.

## O pipeline

```
01 ds-discovery → 02 ds-sketch → 03 ds-architecture → 04 ds-plan → 05 ds-build → 06 ds-qa → 07 ds-ship
```

| Fase | O que produz | Gate humano |
|---|---|---|
| `ds-discovery` | `brief.md`, `prd.md`, `component-inventory.md`, `tokens.draft.json` | Delivery lead aprova brief/PRD |
| `ds-sketch` | `sketches/` (HTML autocontido), `design-decisions.md` | Cliente/design reviewer escolhe variante |
| `ds-architecture` | `architecture.md` (9 decisões vinculantes) | Delivery lead aprova arquitetura |
| `ds-plan` | `epics/**/task-*.md`, `progress.md` (ledger) | — |
| `ds-build` | Componentes implementados (TDD, revisão em 2 estágios) | — |
| `ds-qa` | `qa-report.md` (a11y, regressão visual, cross-browser) | — |
| `ds-ship` | Docs, changesets, pacotes publicados, `release-notes.md` | Sign-off antes de `npm publish` |

Cada fase roda em **contexto novo**; o estado vive em `.planning/` no repositório do cliente, nunca na conversa. Métodos combinados: BMAD-METHOD (discovery/architecture), GSD (sketch, context engineering) e Superpowers (plan/build: TDD, subagentes, worktrees).

## Estrutura

```
skills/
├── ds-discovery/SKILL.md
├── ds-sketch/SKILL.md
├── ds-architecture/SKILL.md
├── ds-plan/SKILL.md
├── ds-build/SKILL.md
├── ds-qa/SKILL.md
└── ds-ship/SKILL.md
agents/
└── implementer.md   # prompt do subagente implementador (usado por ds-build)
```

## Instalação por runtime

- **Claude Code**: `cp -r skills/* ~/.claude/skills/` (global) ou `cp -r skills/* .claude/skills/` no projeto do cliente (local)
- **Claude Cowork**: copiar as pastas para a raiz do workspace — lidas automaticamente. Necessário para os subagentes de `ds-build`
- **Cursor**: `cp -r skills/* .cursor/skills/` — sem subagentes nativos: rodar as tasks de `ds-build` sequencialmente

Copiar também `agents/` para o projeto do cliente (referenciado por `ds-build`).

## Uso num projeto de cliente

1. Clonar o repo do cliente (trabalha-se na infraestrutura dele; ver nota "Ferramentas — Quem Provê" no vault).
2. Instalar as skills (acima) e invocar `ds-discovery`.
3. Seguir as fases em ordem; cada uma indica a próxima e os gates onde parar.

A documentação de negócio (pacotes, papéis, handoff, governança) vive no vault Obsidian `Factory - DS`.
