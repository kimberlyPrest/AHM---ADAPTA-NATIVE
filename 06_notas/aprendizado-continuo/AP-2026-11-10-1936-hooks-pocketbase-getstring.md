# AP-2026-11-10-1936 — Hooks PocketBase JS: getString() vs get() e reinício pós-apply

- Status: capturado
- Escopo: projeto do cliente
- Task/SPEC: F1-T07 / SPEC 1.3 (motor de qualificação, hook de auditoria)
- Sinal: dois comportamentos de hooks JS no PocketBase/Skip que custaram 2 versões de correção — (1) `r.get('campo')` retorna null/undefined para campo vazio, então a comparação `=== ''` nunca dispara; usar `r.getString('campo')`, que retorna '' de verdade. (2) após `apply_changes`, o backend reinicia e as primeiras requisições falham (HTTP 000/503) ou — pior — são atendidas pelo processo antigo, com hooks desatualizados; um teste imediato pode validar comportamento errado e dar falso negativo/positivo.
- Evidência: hook on_leads_qualificar.js com `get()` não bloqueou qualificado sem justificativa (HTTP 200); corrigido para `getString()` bloqueou (HTTP 400). Teste imediato pós-apply v0.0.18 deu 000/503 e depois um 200 atendido pelo hook antigo; só após health 200 + retry o comportamento novo foi validado.
- Regra reutilizável: em hooks JS do PocketBase, sempre getString/getBool/getNumber para comparação com vazio; após apply_changes, esperar o backend estabilizar (health 200) e reexecutar as provas antes de concluir o resultado.
- Quando aplicar: todo hook de validação/auditoria no projeto (F1-T08+, hooks de oportunidade, transbordo).
- Quando não aplicar: código frontend (não usa hooks).
- Confiança: alta — comportamento reproduzido em 2 versões (0.0.18 falhou, 0.0.18 corrigida + 0.0.19 passou).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
