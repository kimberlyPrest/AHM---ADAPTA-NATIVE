# AP-2026-09-25-1420 — Regras de negócio do catálogo garantidas por hooks PocketBase

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T01 / SPEC 1.1 (RN-1.1, CA-1-002)
- Sinal: validações transversais (produto incompleto não fica Ativo) foram implementadas como hooks `onRecordCreate`/`onRecordUpdate` lançando `BadRequestError`, complementando o `required` do schema
- Evidência: curl contra a API do Skip (produto completo Ativo → 200; incompleto Ativo → 400 com mensagem; obrigatório ausente → 400 com erro por campo); migração 0007 aplicada pelo próprio `skip_project_apply_changes`
- Regra reutilizável: em tasks do Skip, campo obrigatório vira `required` na migration; regra condicional entre campos vira hook onRecordCreate/onRecordUpdate com BadRequestError — nunca só validação no frontend
- Quando aplicar: F1-T02 em diante, sempre que a SPEC trouxer RN que cruza campos (ex.: RN-1.5 duplicidade, RN-1.14 motivo em rejeição)
- Quando não aplicar: regra que depende de contexto de requisição HTTP (usar hook de request) ou de efeito pós-commit (usar onRecordAfter*Success)
- Confiança: alta — comportamento verificado por requisições reais com resposta observada
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
