# AP-2026-11-11-0545 — Filtro de UI precisa da expressão completa, não do valor cru

**Task:** F1-T11 (filtro por status na fila) · **Data:** 2026-11-11 · **Origem:** validação de tela

## O que aconteceu

- O chip de filtro passava o VALOR ("Pendente atribuição") direto ao service, que espera
  EXPRESSÃO de filtro PocketBase ("status='Pendente atribuição'"). A prova via API passou
  (montava a expressão correta); a tela quebrou com toast "Erro ao carregar fila".

## Regra reutilizável

- Camada de UI deve construir a expressão completa (campo='valor') ANTES de chamar o service —
  fronteira UI/service explícita. Service nunca aceita valor cru de filtro.
- Prova de tela com clique real (agent-browser) pega o que a prova de API não pega: rodar as
  duas SEMPRE em tasks com filtro/busca.
