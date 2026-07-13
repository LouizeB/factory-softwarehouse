---
name: ds-audit
description: Fase opcional de entrada do pipeline — auditoria do design system ou UI kit existente do cliente. Diagnostica tokens, componentes, acessibilidade, consistência de API e adoção; produz audit-report.md com recomendação evoluir vs. reconstruir e alimenta o ds-discovery. Usar quando o cliente já tem DS/UI kit.
---

# ds-audit — Auditoria de Design System Existente

Fase opcional que antecede `ds-discovery` quando o cliente **já tem** um design system, UI kit ou biblioteca de componentes. Vendável como serviço avulso (diagnóstico) ou como primeira etapa de um projeto de evolução.

Nome cliente-facing: **Auditoria & Diagnóstico**.

## Pré-requisitos

- Acesso de leitura ao(s) repositório(s) do DS/UI kit atual do cliente.
- Acesso a onde ele é consumido (1-2 produtos reais que o usam) — sem isso a análise de adoção fica cega.
- Figma/Sketch do UI kit, se o "design system" for só design (sem código).

## Dimensões da auditoria

Avaliar cada uma com evidência concreta (arquivo/linha/print), nota 1-5 e recomendação:

1. **Tokens**: existem? São usados de fato ou há valores hardcoded espalhados? Cobrem cor, tipografia, espaçamento, tema escuro? Cruzar contra a arquitetura de tokens de `references/tokens-schema.md` (4 níveis: primitivo → semântico → componente → override de marca) — sinalizar se o cliente conecta componente direto a valor primitivo, sem camada semântica.
2. **Inventário real vs. declarado**: quais componentes existem, quais são usados de verdade nos produtos (buscar imports), quais estão duplicados ou abandonados. Cruzar contra `references/component-taxonomy.md` para achar lacunas P0 que nem chegaram a ser tentadas — isso vira argumento de escopo, não só de qualidade.
3. **Acessibilidade**: amostragem com axe-core nos componentes mais usados; foco visível, contraste, nomes acessíveis. Usar a seção 6 de `references/audit-checklist.md` como roteiro de verificação.
4. **Consistência de API**: componentes irmãos com convenções divergentes (props/atributos, eventos, nomenclatura). Ver seção 2 e 7 de `references/audit-checklist.md` (Anatomia/API, Web Components).
5. **Arquitetura e distribuição**: como é buildado, versionado e consumido; acoplamento a framework; existe versionamento semântico?
6. **Documentação e adoção**: docs existem e batem com o código? Os times usam o DS ou o contornam (fork local, CSS por cima)?
7. **Divergência design ↔ código**: o Figma/UI kit e o código contam a mesma história?

### Nível de detalhe opcional: auditoria componente a componente

Para diagnósticos mais profundos (ou quando o cliente tem >30 componentes e a nota por dimensão não é granular o suficiente para decidir o que entra no plano de evolução), rodar o checklist completo de `references/audit-checklist.md` (10 seções) por componente e pontuar com `references/maturity-scale.md` (0-5). Preencher uma linha por componente no modelo `skills/ds-audit/assets/audit-inventory-template.csv`, com ação recomendada: manter / refatorar / recriar / consolidar duplicata.

Isso não substitui o sumário executivo — vira anexo (`audit-inventory.csv`) que sustenta a recomendação com evidência componente a componente, útil sobretudo quando "Reconstruir" precisa ser justificado item por item para o cliente.

## Saída — `audit-report.md`

Estrutura em `.planning/audit-report.md`:

- **Sumário executivo** (meia página, linguagem de negócio): estado geral, os 3 problemas mais caros, a recomendação.
- **Diagnóstico por dimensão**: nota, evidências, impacto.
- **Recomendação central — evoluir vs. reconstruir**, com critério explícito:
  - *Evoluir*: fundação aproveitável (tokens razoáveis, base de componentes usada) → plano de evolução incremental por camada.
  - *Reconstruir*: fundação comprometida (sem tokens, APIs caóticas, adoção baixa) → pipeline completo desde ds-discovery, aproveitando o inventário real como insumo.
- **Plano proposto**: fases, o que se aproveita do existente, estimativa por pacote comercial (ver pacote "Migração" além dos 4 padrão).

Nota média de maturidade (se a auditoria componente a componente foi feita) ajuda a calibrar a recomendação — ver interpretação em `references/maturity-scale.md`.

## Gate humano (bloqueante)

Delivery lead revisa o relatório antes da apresentação ao cliente. A recomendação evoluir/reconstruir é decisão comercial tanto quanto técnica.

## Conexão com o pipeline

- **Evoluir**: `audit-report.md` + inventário real alimentam `ds-discovery` (o Analyst o lê como insumo primário; o escopo do PRD vira "lacunas e correções" em vez de "tudo do zero"). O UX-Designer de `ds-discovery` usa `component-taxonomy.md` para confirmar que nada de P0 ficou de fora do plano de correção.
- **Reconstruir**: pipeline normal; o relatório vira o argumento comercial e o inventário real vira o rascunho do `component-inventory.md`.

Próxima fase: **ds-discovery**, em contexto novo.
