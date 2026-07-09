# 🏭 Factory Software House

**Motor de produção da software house de design systems**: pipeline de 7 fases operado por agentes inteligentes, com gates humanos obrigatórios em cada etapa crítica.

---

## 🎯 Visão Geral

Este repositório contém as **skills** (prompts especializados) e **agentes** que orquestram o processo de desenvolvimento de design systems, desde a descoberta até a entrega, garantindo qualidade, documentação e governança em cada fase.

---

## 📋 Pipeline de 7 Fases

```
01 ds-discovery → 02 ds-sketch → 03 ds-architecture → 04 ds-plan → 05 ds-build → 06 ds-qa → 07 ds-ship
```

| Fase | Entrada | Saída Principal | Gate Humano | Responsável |
|------|---------|-----------------|-------------|-------------|
| **ds-audit** *(opcional)* | DS/UI Kit existente | `audit-report.md` — Diagnóstico: evoluir vs reconstruir | Delivery lead | Antes do cliente |
| **ds-discovery** | Briefing do cliente | `brief.md`, `prd.md`, `component-inventory.md`, `tokens.draft.json` | Delivery lead aprova brief/PRD | ✅ Obrigatório |
| **ds-sketch** | PRD + Componentes | `sketches/` (HTML autocontido), `design-decisions.md` | Cliente/design reviewer escolhe variante | ✅ Obrigatório |
| **ds-architecture** | Sketches aprovados | `architecture.md` (9 decisões vinculantes) | Delivery lead aprova arquitetura | ✅ Obrigatório |
| **ds-plan** | Arquitetura | `epics/**/task-*.md`, `progress.md` (ledger) | — | — |
| **ds-build** | Tarefas planejadas | Componentes implementados (TDD, revisão 2-stage) | — | — |
| **ds-qa** | Build completo | `qa-report.md` (a11y, regressão visual, cross-browser) | — | — |
| **ds-ship** | QA aprovado | Docs, changesets, pacotes publicados, `release-notes.md` | Sign-off antes de `npm publish` | ✅ Obrigatório |

### 🔧 Skill de Apoio

- **ds-report** — Gera relatório semanal para o cliente a partir do ledger durante `ds-build`; delivery lead revisa e envia.

---

## 📁 Estrutura do Repositório

```
factory-softwarehouse/
├── README.md                      # Este arquivo
├── LICENSE                        # MIT License
│
├── skills/                        # Prompts especializados por fase
│   ├── ds-audit/SKILL.md         # Auditoria de DS/UI kit existente (opcional)
│   ├── ds-discovery/SKILL.md     # Descoberta, briefing, inventário de componentes
│   ├── ds-sketch/SKILL.md        # Prototipagem em HTML, design decisions
│   ├── ds-architecture/SKILL.md  # Decisões de arquitetura vinculantes
│   ├── ds-plan/SKILL.md          # Planejamento de epics e tarefas
│   ├── ds-build/SKILL.md         # Implementação TDD, code review 2-stage
│   ├── ds-qa/SKILL.md            # Testes: a11y, regressão visual, cross-browser
│   ├── ds-report/SKILL.md        # Relatórios semanais para cliente
│   └── ds-ship/SKILL.md          # Release: docs, changesets, npm publish
│
└── agents/                        # Sub-agentes e prompts de orquestração
    └── implementer.md            # Prompt do sub-agente implementador (usado por ds-build)
```

---

## 🔄 Como Funciona

1. **Contexto Isolado**: Cada fase roda em contexto novo; o estado vive em `.planning/` no repositório do cliente, nunca na conversa
2. **Métodos Combinados**: 
   - BMAD-METHOD (discovery/architecture)
   - GSD (sketch, context engineers)
   - TDD (build)
3. **Gates Humanos**: Delivery lead e cliente revisam decisões críticas em pontos específicos
4. **Ledger**: `progress.md` mantém histórico transacional de decisões durante `ds-build`

---

## 🚀 Instalação por Runtime

### Claude Code
```bash
# Global
cp -r skills/* ~/.claude/skills/

# Ou local (no projeto do cliente)
cp -r skills/* .claude/skills/
```

### Claude Cowork
```bash
# Copiar pastas para a raiz do workspace (lidas automaticamente)
cp -r skills/* ./
cp -r agents/ ./

# Necessário para sub-agentes em ds-build
```

### Cursor
```bash
cp -r skills/* .cursor/skills/

# Nota: Sem sub-agentes nativos — rodar tasks de ds-build sequencialmente
```

---

## 📖 Uso num Projeto de Cliente

### Passo 1: Preparar o Repositório
```bash
git clone <repo-cliente>
cd <repo-cliente>
```

### Passo 2: Instalar Skills e Agentes
- Copiar as skills conforme sua plataforma (acima)
- Copiar também `agents/` para o projeto do cliente

### Passo 3: Executar o Pipeline
1. **Invocar `ds-discovery`** → Segue gates humanos obrigatórios
2. Cada fase indica a próxima e onde parar para revisão
3. Respeitar a ordem: discovery → sketch → architecture → plan → build → qa → ship

---

## 📚 Documentação

- **Negócio, Pacotes, Papéis, Handoff, Governança**: Vault Obsidian `Factory - DS`
- **Cada SKILL.md**: Instruções detalhadas, templates, exemplos de output
- **agents/implementer.md**: Prompt do sub-agente para tarefas de implementação

---

## 📋 Checklist Rápido

- [ ] Clonar repo do cliente
- [ ] Instalar skills para sua plataforma (Code/Cowork/Cursor)
- [ ] Copiar `agents/` para o projeto
- [ ] Invocar `ds-discovery`
- [ ] Seguir gates humanos (delivery lead + cliente)
- [ ] Passar por todas as 7 fases na ordem
- [ ] Sign-off final antes de `npm publish`

---

## ⚙️ Tecnologias & Métodos

- **Agentes Claude**: Orquestração automática do pipeline
- **Métodos**:
  - BMAD-METHOD (Business Model, Architecture, Design)
  - GSD (Getting Stuff Done — prototipagem rápida)
  - TDD (Test-Driven Development — na fase ds-build)
- **Saídas**: Markdown, JSON (tokens), HTML (sketches), código TypeScript

---

## 📞 Suporte

Para questões sobre o pipeline, métodos ou integração:
- Consulte o vault `Factory - DS` (documentação de negócio)
- Revise a SKILL.md correspondente à sua fase
- Valide gates humanos obrigatórios antes de avançar

---

**Criado por**: LouizeB  
**Licença**: MIT  
**Última atualização**: 2026-07-09
