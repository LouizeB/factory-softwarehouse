# Audit Checklist — por componente

Uma passada completa por componente, nas 10 seções abaixo. Usado por [[ds-audit]] no modo B (legado) e como critério de "DONE" mais rigoroso no modo A (gap-check).

## 1. Identificação
- [ ] Nome oficial definido
- [ ] Problema que resolve documentado
- [ ] Quando usar / quando não usar documentado
- [ ] Componentes relacionados identificados
- [ ] Owner definido
- [ ] Status: draft, beta, stable ou deprecated

## 2. Anatomia e API
- [ ] Anatomia e subcomponentes documentados
- [ ] Slots definidos, conteúdo obrigatório/opcional claro
- [ ] Propriedades, atributos, eventos e defaults documentados
- [ ] Composição com outros componentes validada
- [ ] API permite extensão sem duplicação

## 3. Variantes
- [ ] Variantes visuais e semânticas cobertas
- [ ] Tamanhos, orientação (h/v), com/sem ícone
- [ ] Conteúdo curto e longo testado
- [ ] Responsividade, densidade (compact/comfortable)
- [ ] Light, dark e high-contrast (quando aplicável)
- [ ] Todas as marcas suportadas

## 4. Estados
Testar os que se aplicam (decisão de quais aplicam deve estar documentada):
default · hover · focus visible · active/pressed · selected · checked · expanded/collapsed · loading · read-only · disabled · invalid/error · success · warning · dragging · empty · truncated

## 5. Design tokens
- [ ] Zero valor visual hardcoded
- [ ] Usa tokens semânticos (não primitivos direto)
- [ ] Tokens de componente só quando necessário
- [ ] Todos os estados usam tokens
- [ ] Light/dark usam a mesma API semântica
- [ ] Troca de marca não altera markup
- [ ] Tokens com fallback, sincronizados design↔código
- [ ] Breaking changes de token são versionadas

## 6. Acessibilidade (WCAG 2.2 + WAI-ARIA APG para comportamento de widget)
- [ ] HTML semântico nativo sempre que possível
- [ ] Nome/descrição acessível presentes
- [ ] Funciona 100% via teclado, ordem de foco previsível, sem keyboard trap
- [ ] Focus visible perceptível
- [ ] Estados comunicados à tecnologia assistiva; mudanças assíncronas anunciadas
- [ ] Erros identificados e associados ao campo
- [ ] Não depende só de cor; contraste de texto e gráficos aprovado
- [ ] Touch target adequado; zoom 200%/400% e reflow validados
- [ ] `prefers-reduced-motion` e `forced-colors` suportados
- [ ] Ícones decorativos ocultos da árvore de acessibilidade
- [ ] Testado com leitor de tela real
- [ ] Segue o padrão APG do widget correspondente (tabs/menu/dialog/accordion/tree)

## 7. Web Components
- [ ] Custom element com prefixo do sistema
- [ ] API funciona independente de framework
- [ ] Attributes para valores serializáveis, properties para objetos/estados complexos
- [ ] Eventos são CustomEvent, nomes consistentes, atravessam Shadow DOM quando relevante
- [ ] Slots nomeados consistentemente, com fallback
- [ ] CSS Parts expõem só pontos seguros de customização
- [ ] CSS Custom Properties usam tokens do sistema
- [ ] Shadow DOM não prejudica labels/leitura
- [ ] Form controls participam do form (reset/submit/validação funcionam)
- [ ] SSR/hidratação avaliados
- [ ] Funciona em React, Angular, Vue e HTML puro
- [ ] Não depende de CSS global do produto; não quebra aninhado em outro Shadow DOM

## 8. Responsividade e conteúdo
mobile · tablet · desktop · container estreito · container amplo · texto longo · texto traduzido · RTL · conteúdo vazio · grande volume de dados · zoom · portrait/landscape

## 9. Qualidade técnica
- [ ] Testes unitários, de interação e de teclado
- [ ] Testes automatizados de a11y (ex: axe) — não substituem validação manual
- [ ] Testes visuais por tema e por marca; testes de regressão
- [ ] Testado nos navegadores/dispositivos suportados
- [ ] Performance e bundle size acompanhados
- [ ] Sem erro no console, sem vazamento de listener

## 10. Documentação e entrega
- [ ] Documentado no Storybook (exemplo básico + complexo)
- [ ] Guideline de conteúdo, acessibilidade e responsividade
- [ ] Exemplos light/dark e de todas as marcas
- [ ] API, eventos e tokens documentados; do's and don'ts
- [ ] Design e código equivalentes (Code Connect ou equivalente configurado)
- [ ] Changelog atualizado, versão publicada, migração documentada se breaking change
