# 🏭 Factory Software House

**Motor de produção da software house de design systems**: pipeline de 7 fases operado por agentes inteligentes, com gates humanos obrigatórios em cada etapa crítica.

---

## 🎯 Visão Geral

Este repositório contém as **skills** (prompts especializados) e **agentes** que orquestram o processo de desenvolvimento de design systems, desde a descoberta até a entrega, garantindo qualidade e rastreabilidade em cada decisão crítica.

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

Escolha a plataforma que você usa e siga as instruções de instalação correspondente.

### Claude Code

**Global (disponível em todos os projetos):**
```bash
cp -r skills/* ~/.claude/skills/
cp -r agents/ ~/.claude/agents/
```

**Local (apenas no projeto do cliente):**
```bash
cp -r skills/* .claude/skills/
cp -r agents/ .claude/agents/
```

**Ativação:** As skills aparecem automaticamente nas sugestões do Claude Code.

---

### Claude Cowork

**Workspace-level (obrigatório para sub-agentes):**
```bash
# Na raiz do workspace do cliente
cp -r skills/ ./
cp -r agents/ ./

# As pastas são lidas automaticamente pelo Cowork
```

**Configuração:** Declare os agentes no arquivo `cowork.json` (se necessário):
```json
{
  "agents": {
    "implementer": "agents/implementer.md"
  },
  "skills": {
    "ds-discovery": "skills/ds-discovery/SKILL.md",
    "ds-sketch": "skills/ds-sketch/SKILL.md",
    "ds-architecture": "skills/ds-architecture/SKILL.md",
    "ds-plan": "skills/ds-plan/SKILL.md",
    "ds-build": "skills/ds-build/SKILL.md",
    "ds-qa": "skills/ds-qa/SKILL.md",
    "ds-report": "skills/ds-report/SKILL.md",
    "ds-ship": "skills/ds-ship/SKILL.md"
  }
}
```

---

### GitHub Copilot

**Instalação no VS Code/JetBrains:**
```bash
# 1. Clone o repo de skills localmente
git clone https://github.com/LouizeB/factory-softwarehouse ~/factory-softwarehouse

# 2. Configure a extensão Copilot no seu IDE
# VS Code: Copilot Chat → Settings → Custom Instructions
# Adicione o caminho para as skills:
~/factory-softwarehouse/skills/

# 3. Configure o arquivo .copilot/config.json no projeto do cliente
cat > .copilot/config.json << EOF
{
  "custom_instructions": "../factory-softwarehouse/skills",
  "agents": {
    "implementer": "../factory-softwarehouse/agents/implementer.md"
  }
}
EOF
```

**Uso:** Abra o Chat do Copilot e comande:
```
@skills ds-discovery
```

---

### Codex

**Instalação via OpenAI API:**
```bash
# 1. Exporte as skills como prompts de sistema
cat > .codex/system-prompt.txt << 'EOF'
$(cat skills/ds-discovery/SKILL.md)
---
$(cat skills/ds-sketch/SKILL.md)
---
$(cat skills/ds-architecture/SKILL.md)
---
$(cat skills/ds-plan/SKILL.md)
---
$(cat skills/ds-build/SKILL.md)
---
$(cat skills/ds-qa/SKILL.md)
---
$(cat skills/ds-report/SKILL.md)
---
$(cat skills/ds-ship/SKILL.md)
EOF

# 2. Configure a chamada à API do Codex com system prompt
export CODEX_SYSTEM_PROMPT=$(cat .codex/system-prompt.txt)
```

**Exemplo de chamada com curl:**
```bash
curl https://api.openai.com/v1/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "code-davinci-002",
    "prompt": "'"$(cat .codex/system-prompt.txt)"'\n\n[Seu comando aqui]",
    "max_tokens": 2048,
    "temperature": 0.3
  }'
```

**Via Node.js:**
```javascript
import { OpenAI } from "openai";
import fs from "fs";

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });
const systemPrompt = fs.readFileSync(".codex/system-prompt.txt", "utf-8");

async function callCodex(userMessage) {
  const response = await client.chat.completions.create({
    model: "gpt-3.5-turbo-16k",
    system: systemPrompt,
    messages: [{ role: "user", content: userMessage }],
    max_tokens: 2048,
  });
  return response.choices[0].message.content;
}

callCodex("Inicie a fase de ds-discovery...");
```

---

### Devin (Cognition Labs)

**Instalação via plataforma Devin:**
```bash
# 1. Importe o repositório no Devin
# Dashboard → Projects → Import from GitHub
# https://github.com/LouizeB/factory-softwarehouse

# 2. Configure o prompt de sistema
# Project Settings → Custom Instructions
# Cole o conteúdo combinado de todas as skills

# 3. Configure sub-agentes
# Settings → Agents → Add Agent
# Nome: implementer
# Prompt: agents/implementer.md
```

**Arquivo de configuração `.devin/config.json`:**
```json
{
  "project_name": "Factory DS Pipeline",
  "system_prompt_from_file": "skills/ds-discovery/SKILL.md",
  "agents": [
    {
      "name": "analyst",
      "role": "discovery-analyst",
      "prompt_file": "skills/ds-discovery/SKILL.md"
    },
    {
      "name": "implementer",
      "role": "component-builder",
      "prompt_file": "agents/implementer.md"
    }
  ],
  "workflows": [
    {
      "name": "design-system-pipeline",
      "phases": [
        "ds-discovery",
        "ds-sketch",
        "ds-architecture",
        "ds-plan",
        "ds-build",
        "ds-qa",
        "ds-ship"
      ]
    }
  ],
  "gates": {
    "before_sketch": ["brief.md", "prd.md"],
    "before_architecture": ["sketches", "design-decisions.md"],
    "before_build": ["architecture.md"],
    "before_ship": ["qa-report.md"]
  }
}
```

**Uso no Devin:**
```
@devin execute-workflow design-system-pipeline
```

---

### Cursor

**Instalação local:**
```bash
cp -r skills/* .cursor/skills/

# Nota: Cursor não suporta sub-agentes nativamente
# As tasks de ds-build devem rodar sequencialmente
```

**Configuração `.cursor/config.json`:**
```json
{
  "rules": ".cursorules",
  "custom_instructions": "skills/",
  "sequential_mode": true
}
```

---

## 📖 Uso num Projeto de Cliente

### Passo 1: Preparar o Repositório
```bash
git clone <repo-cliente>
cd <repo-cliente>
```

### Passo 2: Instalar Skills e Agentes
- Escolha seu runtime acima
- Copiar as skills conforme sua plataforma
- Copiar também `agents/` para o projeto do cliente (exceto Codex/Cursor)

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
- [ ] Escolher runtime (Claude Code / Cowork / Copilot / Codex / Devin / Cursor)
- [ ] Instalar skills conforme sua plataforma
- [ ] Copiar `agents/` para o projeto (se suportado)
- [ ] Invocar `ds-discovery`
- [ ] Seguir gates humanos (delivery lead + cliente)
- [ ] Passar por todas as 7 fases na ordem
- [ ] Sign-off final antes de `npm publish`

---

## ⚙️ Tecnologias & Métodos

- **Agentes Claude / Copilot / Codex / Devin**: Orquestração automática do pipeline
- **Métodos**:
  - BMAD-METHOD (Business Model, Architecture, Design)
  - GSD (Getting Stuff Done — prototipagem rápida)
  - TDD (Test-Driven Development — na fase ds-build)
- **Saídas**: Markdown, JSON (tokens), HTML (sketches), código TypeScript

---

## 🔄 Comparativo de Runtimes

| Runtime | Sub-agentes | Contexto Isolado | Ledger | Recomendação |
|---------|------------|-----------------|--------|--------------|
| **Claude Code** | ❌ | ✅ Manual | Manual | Solo dev, prototipagem rápida |
| **Claude Cowork** | ✅ Full | ✅ Automático | Automático | **Recomendado principal** |
| **GitHub Copilot** | ⚠️ Limitado | ✅ Manual | Manual | Integração VS Code/IDE |
| **Codex** | ❌ | ✅ Manual | Manual | APIs custom, automação |
| **Devin** | ✅ Full | ✅ Automático | Automático | Autonomia máxima, end-to-end |
| **Cursor** | ❌ | ✅ Manual | Manual | Sequencial, lightweight |

---

## 📞 Suporte

Para questões sobre o pipeline, métodos ou integração:
- Consulte o vault `Factory - DS` (documentação de negócio)
- Revise a SKILL.md correspondente à sua fase
- Valide gates humanos obrigatórios antes de avançar
- Verifique a configuração do seu runtime acima

---

**Criado por**: LouizeB  
**Licença**: MIT  
**Última atualização**: 2026-07-09
