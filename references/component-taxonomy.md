# Component Taxonomy — Referência Mestre

Taxonomia de referência para dois usos: (1) checar se `component-inventory.md` (gerado em [[01 ds-discovery]]) cobre o essencial antes de seguir pro build; (2) mapear componentes de um design system legado durante auditoria (`ds-audit` modo B).

Prioridades:
- **P0** — essencial para lançar os primeiros produtos
- **P1** — necessário para escalar produtos e jornadas
- **P2** — avançado, entra conforme demanda real
- **Especializado** — só criar com demanda comprovada

## 0. Foundations obrigatórias (antes de qualquer componente)

Sem essas fundações, componentes criam valores locais e o sistema deixa de ser multimarcas/multitema.

| Fundação | Cobertura mínima |
|---|---|
| Cores primitivas | Neutras, marca, suporte, dados, transparências |
| Cores semânticas | background, surface, text, icon, border, interactive, focus, success, warning, danger, information |
| Tipografia | Família, peso, tamanho, line-height, letter-spacing, estilos semânticos |
| Espaçamento | Escala única |
| Dimensionamento | Alturas, larguras, ícones, controles |
| Bordas / Border radius | Largura, estilo, cor, escala de raios |
| Elevação | Sombras, overlays, níveis de superfície |
| Opacidade | Disabled, overlay, subtle, skeleton |
| Motion | Duração, easing, entrada/saída/transição |
| Breakpoints / Grid | Mobile, tablet, desktop, wide; colunas, gutters, margens |
| Densidade | Comfortable, compact (spaceous se necessário) |
| Z-index | Conteúdo, sticky, dropdown, overlay, modal, toast, tooltip |
| Iconografia / Ilustração | Tamanhos, stroke, regras de uso por contexto |
| Focus | Cor, largura, offset, comportamento |
| Data visualization | Paletas categórica, sequencial, divergente, estados |
| Acessibilidade | Contraste, zoom, teclado, motion, touch target |
| Internacionalização | RTL, expansão de texto, moeda, datas, idiomas |
| Conteúdo | Labels, mensagens, erros, tom de voz |

**Arquitetura de tokens (mínimo 4 níveis):** `primitivo → semântico → componente → override de marca`. Nunca conectar componente direto a valor primitivo.

**Eixos de variação, tratados separadamente (não misturar num tema só):** Marca × Tema (light/dark/high-contrast) × Densidade × Plataforma × Direção (LTR/RTL).

## 1. Ações
P0: Button, Icon Button, Link
P1: Button Group, Toggle Button, Toggle Group, Menu Button, Split Button
P2: Floating Action Button, Copy Button, Share Button

Variantes mínimas de Button: primary/secondary/tertiary-ghost/danger, icon-only, com ícone inicial/final, full-width, loading, 3 tamanhos.

## 2. Formulários e entradas
P0: Form Field, Label, Helper Text, Error Message, Text Input, Textarea, Password Input, Search Input, Checkbox (+Group), Radio (+Group), Switch, Select
P1: Native Select, Combobox, Autocomplete, Multiselect, Number Input, Date/Time Picker, Date Range Picker, File Upload, Dropzone, Slider (+Range), Input Group, Prefix/Suffix, Character Counter
P2: OTP Input, Currency/Percentage/Phone/Masked Input, Rich Text Editor
Especializado: Address Input, Rating, Color Picker, Signature Input

`Form Field` é estrutura compartilhada, não decoração: Label + Required indicator + Control + Helper text + Validation message + Character counter + Tooltip.

## 3. Navegação
P0: App Header, Navigation Menu, Sidebar, Breadcrumb, Tabs, Pagination, Skip Link
P1: Page Header, Subnavigation, Anchor Navigation, Stepper, Progress Indicator, Command Palette, Overflow Menu, Dropdown/Mobile/Bottom Navigation
P2: Mega Menu, Tree Navigation, Back to Top, Navigation Rail, Recent Items, Favorites Navigation

Sidebar precisa contemplar: expandido/recolhido, itens aninhados, grupos, badges, estado atual, teclado, responsividade, persistência de estado.

## 4. Feedback, status e comunicação
P0: Alert, Inline Notification, Toast, Badge, Tag, Status Indicator, Spinner, Progress Bar, Skeleton, Empty State, Error State
P1: Banner, Actionable Notification, Loading Overlay, Inline Loading, Success/No Results/Offline/Maintenance State, Connection Status, Tooltip, Toggletip
P2: Notification Center, Announcement Bar, Upload Progress, Multi-step Progress, Activity Indicator

Separar semanticamente Alert / Toast / Banner / Inline Notification / Alert Dialog — diferem em interrupção, persistência e necessidade de resposta.

## 5. Overlays e conteúdo flutuante
P0: Dialog/Modal, Alert Dialog, Dropdown Menu, Popover, Tooltip
P1: Drawer, Sheet, Context Menu, Confirmation Dialog, Full-screen Dialog, Hover Card, Date Picker Popover
P2: Lightbox, Coachmark, Guided Tour, Preview Panel, Nested Menu

Auditar sempre: gerenciamento de foco, fechamento com Escape, devolução de foco, bloqueio de interação externa (modal), ordem de leitura, scroll interno/externo, comportamento mobile, anúncio por tecnologia assistiva.

## 6. Exibição de conteúdo
P0: Typography, Heading, Paragraph, List, Divider/Separator, Card, Avatar, Icon, Image
P1: Accordion, Collapsible, Tile, Contained List, List Item, Description List, Metadata, Stat, KPI Card, Code Snippet, Blockquote, Aspect Ratio, Carousel, Timeline
P2: Feed, Comment, Activity Log, Gallery, Media Object, Rating Display, Comparison Card, Pricing Card

## 7. Tabelas e visualização de dados
P0: Table, Data Table, Table Pagination, Column Sorting, Row Selection, Table Empty State
P1: Table Toolbar/Search/Filters, Bulk Actions, Expandable Row, Column Visibility/Resizing, Sticky Header, Density Control, Export Action, Data Grid, Chart Container/Legend/Tooltip/Empty/Loading State
P2: Tree Table, Editable/Virtualized/Pivot Table, Advanced Filter Builder, Saved Views, Dashboard Widget

Gráficos — P1: Line, Bar, Stacked Bar, Area, Pie/Donut, Sparkline, Progress/Goal Chart. P2: Scatter, Bubble, Heatmap, Histogram, Treemap, Sankey, Gauge. Especializado: Maps/geospatial.

Todo gráfico precisa de: legenda, tooltip, tabela alternativa, descrição, estados sem dados/carregando/erro, seleção, navegação por teclado — não só o SVG.

## 8. Estrutura e layout
P0: Container, Stack, Inline, Grid, Flex, Spacer, Responsive Visibility, Page Layout, App Shell
P1: Header, Footer, Main Content, Section, Toolbar, Side Panel, Split Layout, Scroll Area, Sticky Region, Resizable Panels, Responsive Drawer
P2: Masonry, Master-detail Layout, Dashboard Layout, Workspace Layout

## 9. Identidade, pessoas e entidades
P0: Avatar, Avatar Group
P1: User Menu, Entity Header, Account/Organization Selector, Brand/Product Logo, Flag
P2: Presence Indicator, Profile/Organization Card, Identity Verification Status

## 10. Busca e filtros
P0: Search Input
P1: Global Search, Search Results, Filter (+Group/Chip), Applied Filters, Filter Panel, Sort Control, Result Count, Recent Searches
P2: Advanced Search, Query Builder, Saved Search, Search Suggestions/Highlight, Faceted Navigation

## 11. Arquivos e documentos
P1: File Uploader, Dropzone, File Item/List, Upload Progress, File Error, Download Action
P2: File Preview, Document Viewer, Attachment, Version History, Folder Tree

## 12. Utilitários acessíveis (nem sempre no Figma, sempre no código)
P0: Visually Hidden, Skip Link, Focus Ring, Focus Trap, Live Region, Accessible Icon/Label, Keyboard Navigation Helpers
P1: Portal, Dismissible Layer, Roving Tabindex, Scroll Lock, Reduced Motion Helper, High Contrast Support, Direction Provider, Theme Provider, Brand Provider

## 13. Componentes de IA (quando aplicável — extensão do sistema, não sistema isolado)
P1: AI Label, AI-generated Content Indicator, Explainability Popover, Prompt Input, Chat/User/Assistant Message, Message Actions, Streaming Response, Stop/Regenerate Action, Feedback +/-, Source/Citation, Attachment, Suggested Prompt, AI Loading/Error State, Human Handoff
P2: Tool Call, Reasoning Summary, Model Selector, Conversation History, AI Comparison, Confidence Indicator, Generated Form Fields, AI-edited Content Diff

## Ordem recomendada de construção

**Fase 1 — Fundação e primeiros produtos:** tokens/temas → Typography → Icon → Button/Icon Button → Link → Text Input/Textarea → Checkbox/Radio/Switch → Select → Form Field → Alert/Toast → Dialog → Tooltip → Card → Tabs → Breadcrumb → App Header → Sidebar → Table/Data Table → Pagination → Spinner/Progress/Skeleton → Empty/Error State.

**Fase 2 — Escala de jornadas:** Combobox/Multiselect → Date/Time Picker → File Upload → Drawer/Sheet → Popover/menus → Stepper → Search/filtros → Table Toolbar/bulk actions → Layout primitives → Charts/KPI cards → Notification Center → Command Palette.

**Fase 3 — Produtos avançados:** Data Grid → Tree View/Table → Rich Text Editor → Resizable Panels → Saved Views → Advanced Filter Builder → Guided Tour → Document Viewer → Componentes de IA → Componentes específicos de domínio.

Regra: não construir tudo antes de ter produtos consumidores. Inventário amplo, implementação por demanda/recorrência/risco/impacto.
