# Matriz de Rastreabilidade — AHM

**Atualizada em:** 2026-09-17  
**Fonte soberana:** `03-Projeto/02-Escopo-Definitivo.md`  
**Status do projeto:** aprovado por cliente, CSM e consultora.

| ID | Fonte / decisão | Requisito | Fase | Evidência / aceite |
|---|---|---|---|---|
| R-01 | Projeto aprovado | Seguir para SPECs e tasks sem novo gate de escopo | Todas | escopo definitivo marcado como aprovado |
| R-02 | Fase 1 sem integrações | Permitir cadastro/importação manual e operação local | Fase 1 | nenhuma task da Fase 1 exige API, webhook ou credencial |
| R-03 | AHM.pdf: K1 >= 70% | Medir qualificação automática no recorte piloto | Fase 1, Fase 5 | painel/baseline com numerador e denominador |
| R-04 | AHM.pdf: K2 <= 4 interações | Registrar interações humanas até proposta | Fase 1, Fase 5 | relatório comparando baseline e resultado |
| R-05 | AHM.pdf: K3 >= 1,30x capacidade | Medir leads processados por vendedor | Fase 1, Fase 5 | métrica antes/depois rastreável |
| R-06 | Última reunião: produto precisa de ICP/perguntas/timing | Criar catálogo comercial por produto/família | Fase 1 | produto cadastrado com ICP, perguntas, timing e especialista |
| R-07 | Última reunião: transbordo técnico | Roteamento para especialista | Fase 1, Fase 3 | motivo de transbordo registrado |
| R-08 | Última reunião: sugestão antes de contato | Fila de oportunidades com decisão humana | Fase 1, Fase 3, Fase 5 | aprovar/rejeitar/ajustar registrado |
| R-09 | Última reunião: APIs Pipedrive/RD/Envia | Integrações comerciais posteriores | Fase 2 | lead integrado cria/atualiza fluxo sem quebrar fallback manual |
| R-10 | Última reunião: histórico apoia recompra | Recomendações de recompra/expansão | Fase 3 | oportunidade mostra fonte e justificativa |
| R-11 | Demonstração Ethos/Bart | Loops e skills com limite de autonomia | Fase 4, Fase 5 | loop com meta, cadência, fonte e veredito humano |
| R-12 | Contrato das cinco fases | Validar fases 1-5 no encerramento | Fase 5 | matriz de validação preenchida |

## Rastreabilidade Fase 1 — SPECs e tasks

| Requisito | SPEC | Tasks | Aceite / prova |
|---|---|---|---|
| R-06 | SPEC 1.1 — Catálogo de produtos | F1-T01, F1-T02, F1-T03 | CA-1-001 a CA-1-003 |
| R-02 | SPEC 1.2 — Leads manuais/importação | F1-T04, F1-T05, F1-T06 | CA-1-004 a CA-1-006 |
| R-03 | SPEC 1.3 — Motor de qualificação | F1-T07, F1-T08, F1-T09 | CA-1-007 a CA-1-009 |
| R-07 | SPEC 1.4 — Transbordo especialista | F1-T10, F1-T11, F1-T12 | CA-1-010 a CA-1-012 |
| R-08 | SPEC 1.5 — Fila de oportunidades | F1-T13, F1-T14, F1-T15 | CA-1-013 a CA-1-015 |
| R-03/R-04/R-05 | SPEC 1.6 — Painel K1/K2/K3 | F1-T16, F1-T17, F1-T18 | CA-1-016 a CA-1-018 |
| R-04 | SPEC 1.7 — Interações até proposta | F1-T19, F1-T20, F1-T21 | CA-1-019 a CA-1-021 |

## Fora de escopo rastreado

| Item | Origem | Tratamento |
|---|---|---|
| Omie, fiscal, DANFE e tributação | Recorte aprovado | fora deste ciclo |
| Contato 100% autônomo sem aprovação | Decisão de começar assistido | somente após validação e aceite |
| Decisão técnica complexa por IA generalista | Risco de alucinação discutido na reunião | transbordo obrigatório |
| API como bloqueador de Fase 1 | Decisão da consultora | proibido na Fase 1 |
