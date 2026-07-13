# Maturity Scale — por componente

| Nota | Situação |
|---|---|
| 0 — Ausente | Não existe em design nem código |
| 1 — Fragmentado | Existe isoladamente em algum produto |
| 2 — Candidato | Existe, mas não foi padronizado |
| 3 — Beta | Possui design, código e documentação parcial |
| 4 — Estável | Possui tokens, acessibilidade, testes e documentação |
| 5 — Multimarca | Estável em todas as marcas, temas e produtos |

## Regra para nota 4 (Estável)
Só marcar como estável se TODOS presentes:
design + código · tokens · light + dark · teclado · semântica acessível · testes · documentação · responsividade · owner · versionamento

## Regra para nota 5 (Multimarca)
Só marcar como multimarca se a MESMA api/estrutura/comportamento funcionar em todas as marcas, mudando apenas tokens, assets e configurações permitidas — nunca o markup.

## Uso no relatório de auditoria
Nota média do inventário indica saúde geral do design system existente:
- **0-1.5**: reconstrução do zero é mais barata que migração — tratar como novo projeto ([[01 ds-discovery]] padrão)
- **1.5-3**: migração seletiva — reconstruir P0 quebrados, aproveitar estrutura de tokens se existir
- **3-4**: consolidação — maior parte do trabalho é padronização/tokens/a11y, não recriação visual
- **4+**: manutenção/extensão — pipeline entra só para lacunas (componentes ausentes) e evolução
