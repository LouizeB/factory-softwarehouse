---
name: ds-discovery
description: Fase 1 do pipeline de design system — descoberta e briefing. Roda 3 agentes em sequência (Analyst, PM, UX-Designer) para produzir brief.md, prd.md, component-inventory.md e tokens.draft.json. Usar quando iniciar um projeto novo de design system para um cliente.
---

# ds-discovery — Descoberta & Briefing

Fase 1 do pipeline. Três agentes em sequência, **cada um consumindo só o artefato do anterior** (nunca a conversa inteira). Todos os artefatos vão em `.planning/` na raiz do projeto do cliente.

Nome cliente-facing: **Descoberta & Briefing**.

## Pré-requisitos

- Repositório do cliente clonado (trabalha-se no repo do cliente, não da agência).
- Insumos da call de discovery: anotações, brand guide (se existir), referências citadas.
- Se `.planning/brief.md` já existe e foi aprovado, não refazer — seguir para o passo 2 ou 3 conforme o que falta.

## Passo 1 — Agente Analyst → `brief.md`

Despachar um subagente com APENAS os insumos brutos da call (não o histórico da conversa). Missão: extrair intenção de negócio e restrições de marca. O brief deve responder:

- Existe brand guide? Onde? O que ele fixa (cores, tipografia, tom)?
- Quais times/produtos vão consumir o design system? Em que stacks?
- Referências citadas pelo cliente (Material, Radix, Carbon, outro DS)? O que gostam/rejeitam nelas?
- Restrições duras: acessibilidade exigida, browsers suportados, prazo, orçamento do pacote vendido.

Saída: `.planning/brief.md`.

## Passo 2 — Agente PM → `prd.md`

Despachar subagente novo que lê **somente `.planning/brief.md`**. Missão: transformar intenção em escopo. O PRD deve conter:

- Escopo v1 (o que entra) e fora de escopo explícito (o que NÃO entra) — alinhado ao pacote vendido (Foundations / Core / Full DS).
- Critérios de sucesso mensuráveis.
- Fases de entrega mapeadas nas 4 camadas: foundations → primitives → composite → patterns.

Saída: `.planning/prd.md`.

## Passo 3 — Agente UX-Designer → inventário + tokens

Despachar subagente novo que lê **`brief.md` + `prd.md`**. Missão: inventário de componentes por camada. Para cada componente:

- Camada (Foundations / Primitives / Composite / Patterns)
- Estados obrigatórios (default, hover, focus, active, disabled, error, loading — os que se aplicam)
- Variantes (ex.: button → primary/secondary/ghost; tamanhos)
- Dependências de outros componentes

Saídas:
- `.planning/component-inventory.md`
- `.planning/tokens.draft.json` — rascunho de tokens no formato de categorias que a fase ds-architecture vai converter em custom properties `--ds-<categoria>-<propriedade>-<escala>`

## Gate humano (bloqueante)

**Parar aqui.** O delivery lead precisa aprovar `brief.md` e `prd.md` antes de qualquer fase seguinte — é o que define o que foi vendido. Registrar a aprovação (data + quem) no topo do próprio `prd.md`.

## Saída da fase

Artefatos em `.planning/`: `brief.md`, `prd.md` (aprovados), `component-inventory.md`, `tokens.draft.json`.
Próxima fase: **ds-sketch**, em contexto novo.
