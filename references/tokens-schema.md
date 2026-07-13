# Tokens Schema

`tokens/tokens.json` é a única fonte de verdade para valores visuais. Nenhum componente deve ter cor, espaçamento, raio, sombra ou tipografia hardcoded — sempre via `var(--ds-...)`.

## Formato

```json
{
  "color": {
    "bg": { "primary": "#0b0b0c", "secondary": "#1a1a1c" },
    "text": { "primary": "#f5f5f5", "muted": "#a1a1aa" },
    "border": { "default": "#2a2a2d", "focus": "#5b8def" },
    "feedback": { "danger": "#e5484d", "success": "#30a46c", "warning": "#f5a524" }
  },
  "space": { "1": "4px", "2": "8px", "3": "12px", "4": "16px", "6": "24px", "8": "32px" },
  "radius": { "sm": "4px", "md": "8px", "lg": "16px", "full": "9999px" },
  "typography": {
    "family": { "base": "Inter, system-ui, sans-serif", "mono": "'JetBrains Mono', monospace" },
    "size": { "sm": "13px", "base": "15px", "lg": "18px", "xl": "24px" },
    "weight": { "regular": "400", "medium": "500", "bold": "700" }
  },
  "motion": { "fast": "120ms", "base": "200ms", "slow": "320ms" },
  "elevation": {
    "1": "0 1px 2px rgba(0,0,0,0.08)",
    "2": "0 4px 12px rgba(0,0,0,0.12)"
  }
}
```

## Geração de CSS

Cada chave vira uma custom property com prefixo `--ds-`, achatando o path:
`color.bg.primary` → `--ds-color-bg-primary`
`space.4` → `--ds-space-4`

Um script (`packages/tokens/build.ts`) faz essa transformação automaticamente — nunca escreva `--ds-*` à mão fora desse gerador.

## Temas

Tema escuro/claro = mesmo conjunto de chaves, valores diferentes, aplicado via `[data-theme="dark"] { ... }` sobrescrevendo as custom properties na raiz. Componentes nunca sabem qual tema está ativo — só consomem a variável.
