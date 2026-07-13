# Component Conventions

Convenções obrigatórias para qualquer componente gerado em `ds-build`.

## Nomenclatura

- Tag custom: `ds-<nome>` (ex: `ds-button`, `ds-dropdown-menu`)
- Classe: `Ds<Nome>` em PascalCase (ex: `DsButton`)
- Arquivo: kebab-case (ex: `dropdown-menu.ts`)

## Base

Lit (`LitElement`), a menos que `architecture.md` diga o contrário para um pacote específico.

```ts
import { LitElement, html, css } from 'lit';
import { customElement, property } from 'lit/decorators.js';

@customElement('ds-button')
export class DsButton extends LitElement {
  static styles = css`
    :host {
      display: inline-flex;
      --_bg: var(--ds-color-bg-primary);
    }
  `;

  @property({ type: String, reflect: true }) variant: 'primary' | 'secondary' = 'primary';
  @property({ type: Boolean, reflect: true }) disabled = false;

  render() {
    return html`<button ?disabled=${this.disabled}><slot></slot></button>`;
  }
}
```

## Atributos vs. propriedades

- Valores primitivos (string/number/boolean) que fazem sentido no HTML estático → atributo refletido (`reflect: true`)
- Objetos/arrays complexos → propriedade apenas (setada via JS), nunca serializada como atributo

## Eventos

- Sempre `CustomEvent`, sempre `bubbles: true, composed: true` (atravessa shadow boundary)
- Nome em kebab-case prefixado: `ds-change`, `ds-open`, nunca reutilize nomes nativos (`change`, `click`) para eventos com payload customizado

```ts
this.dispatchEvent(new CustomEvent('ds-change', {
  detail: { value: this.value },
  bubbles: true,
  composed: true,
}));
```

## Shadow DOM

- `open` sempre (permite `element.shadowRoot` para testes e ferramentas de a11y)
- Estilização externa via `::part()` para os elementos internos que precisam ser customizáveis pelo consumidor — declare quais partes são públicas na task/API do componente, o resto é encapsulado

## Estados via atributo, nunca via classe

```css
:host([disabled]) { opacity: 0.5; pointer-events: none; }
:host(:focus-visible) { outline: 2px solid var(--ds-color-border-focus); }
```

## Registro seguro

```ts
if (!customElements.get('ds-button')) {
  customElements.define('ds-button', DsButton);
}
```
Evita erro caso o pacote seja importado duas vezes (comum em monorepos com múltiplos bundlers).

## Acessibilidade

- Todo componente interativo precisa de `:focus-visible` visível e navegação por teclado completa antes de ser considerado DONE
- Roles ARIA explícitos quando o elemento não é semanticamente nativo (ex: `ds-tabs` precisa de `role="tablist"` nos slots internos)
