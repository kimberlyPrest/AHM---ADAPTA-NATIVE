# Changelog

- 2026-11-10 · [Afonso] · Task F1-T10 IMPLEMENTADA (aguardando teste humano): fila de transbordo (SPEC 1.4). Migration 0018_create_transbordos (coleção transbordos: lead, produto, motivo, especialista, status [Pendente atribuição|Aberto|Resolvido], comentario, resolucao, resolvido_em/por + índices). Hook on_transbordo_validate.js (RN-1.11: motivo obrigatório; RN-1.12: resolução vazia não fecha; auditoria resolvido_em/por via hook de requisição). Hook on_leads_qualificar.js estendido (RN-1.11: qualificação marcando transbordo cria o item na fila automaticamente, uma vez por transição, com especialista herdado do produto; sem especialista → "Pendente atribuição"). Tela /transbordos (fila paginada server-side 20/página, contadores, modal atribuir especialista + resolver com resolução obrigatória) + rota e item no menu. Skip 0.0.29 e 0.0.30 (hash a290e86), QA completo OK. Provas via API (12/12): CA-1-010 item criado automaticamente com motivo; cenário limite especialista herdado; RN-1.11 sem motivo → 400; RN-1.12 resolução vazia → 400, com resolução → 200 + resolvido_em/por; regressão: qualificação normal não cria item na fila. Base limpa após provas (0 transbordos, 0 leads de prova). Correção em 0.0.30: resolvido_por exigia hook de requisição (model hook não tem e.auth).

## 2026-09-17

- Pasta operacional inicial criada para a Fase 1.
