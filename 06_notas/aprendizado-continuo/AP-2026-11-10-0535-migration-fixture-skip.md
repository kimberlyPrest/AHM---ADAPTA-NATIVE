# AP-2026-11-10-0535 — Migration de fixture no Skip: try/catch + ordem de replay

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T03 / EMENDA 1.1a (fixture Shockwatch + coleções modelos_negocio/templates_diagnostico)
- Sinal: duas armadilhas de migration com fixture no Skip/PocketBase — (1) `findRecordById` com id fixo digitado na migration lança `sql: no rows in result set` em base nova (o QA integrations rejeita); resolver com try/catch e no-op. (2) migration de fixture pode rodar antes dos seeds terem efeito no replay — os vínculos (relations) não são aplicados; resolver aplicando cada vínculo só se o lookup encontrar o registro E corrigindo a base ativa por PATCH quando necessário; a migration robusta reaplica em replay.
- Evidência: QA integrations rejeitou 0.0.6 (findRecordById sem try/catch); v0.0.7 passou o QA mas vínculos vazios na base ativa (migration rodou antes dos seeds); PATCH de correção aplicou vínculos; v0.0.8 com lookups tolerantes passou QA e as 9/9 provas do fixture.
- Regra reutilizável: em migrations de fixture no Skip, nunca confiar em registro de id fixo sem try/catch; aplicar cada vínculo condicionalmente ao lookup; após apply_changes, revalidar o fixture na base ativa via API (expand) e corrigir por PATCH se a ordem de replay deixou vínculos vazios.
- Quando aplicar: toda migration que popula dados ou vincula registros no Skip (F1-T04 leads, F1-T18 baseline 10 leads, seeds futuros).
- Quando não aplicar: migrations de schema puro (criar coleção/campo) — não dependem de registros existentes.
- Confiança: alta — comportamento observado em 3 versões consecutivas (0.0.6 rejeitada, 0.0.7 vínculos vazios, 0.0.8 correta).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
