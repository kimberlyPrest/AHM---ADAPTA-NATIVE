# AP-2026-11-11-0600 — Efeito em registro vinculado precisa ser visível onde o usuário está

**Task:** F1-T12 (resolução devolve lead) · **Data:** 2026-11-11 · **Origem:** teste humano

## O que aconteceu

- A resolução do transbordo devolvia o lead a "pendente" com justificativa auditável (prova via
  API confirmava), mas o usuário NÃO CONSEGUIA VER: o modal não mostrava a situação do lead e em
  Leads (15 mil registros) exigiria busca manual. Owner: "por que não consigo identificar se
  continua pendente ou não".

## Regra reutilizável

- Quando uma ação altera um registro VINCULADO (transbordo → lead), a tela onde o usuário está
  DEVE mostrar o estado resultante do registro vinculado (seção "Situação do lead").
- Prova de API confirma o dado; prova de UX confirma a PERCEPÇÃO. Em tasks com efeito em
  registro vinculado, as duas são obrigatórias — a segunda é a que o cliente testa.
