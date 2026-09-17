# Fase 1 — Tasks gerais

**Regra da fase:** nenhuma task depende de API, credencial, webhook, conector, integração externa ou decisão pendente do cliente.

## Tasks

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T01 | Criar modelo e validações do catálogo de produtos | Ethos | SPEC 1.1 | Campos obrigatórios impedem produto incompleto ativo | CA-1-001, CA-1-002 / RED+GREEN | log/captura de produto salvo e erro obrigatório | nenhuma | ☐ |
| F1-T02 | Criar tela/lista de catálogo com ativar/inativar | Ethos | SPEC 1.1 | Produto ativo aparece; inativo some de nova seleção | CA-1-001, CA-1-003 / GREEN+REGRESSÃO | captura da lista antes/depois | F1-T01 | ☐ |
| F1-T03 | Criar fixture de produto exemplo para demonstração | Ethos | SPEC 1.1 | Produto exemplo sustenta qualificação manual | CA-1-001 / demonstração | captura do produto exemplo | F1-T02 | ☐ |
| F1-T04 | Criar modelo e formulário de lead manual | Ethos | SPEC 1.2 | Lead completo pode ser criado como novo | CA-1-004 / GREEN | captura/log do lead criado | nenhuma | ☐ |
| F1-T05 | Criar importação CSV com prévia e relatório de erros | Ethos | SPEC 1.2 | Válidos importam e inválidos são reportados | CA-1-005 / GREEN+erro | relatório de importação | F1-T04 | ☐ |
| F1-T06 | Criar sinalização de duplicidade local | Ethos | SPEC 1.2 | Duplicidade contato+empresa é avisada antes de confirmar | CA-1-006 / REGRESSÃO | captura do aviso | F1-T04 | ☐ |
| F1-T07 | Implementar estados e regras iniciais de qualificação | Ethos | SPEC 1.3 | Lead completo vira qualificado; incompleto vira pendente | CA-1-007, CA-1-008 / GREEN | captura/log dos status | F1-T01, F1-T04 | ☐ |
| F1-T08 | Implementar justificativa e reprocessamento de qualificação | Ethos | SPEC 1.3 | Resultado mostra motivos e muda após edição | CA-1-007, CA-1-009 / REGRESSÃO | captura/log com motivos | F1-T07 | ☐ |
| F1-T09 | Implementar tratamento de produto inativo/inexistente na qualificação | Ethos | SPEC 1.3 | Qualificação bloqueia produto inválido sem apagar lead | CA-1-009 / erro | captura/log do bloqueio | F1-T07 | ☐ |
| F1-T10 | Criar fila/modelo de transbordo | Ethos | SPEC 1.4 | Lead complexo cria item com motivo | CA-1-010 / GREEN | captura da fila | F1-T07 | ☐ |
| F1-T11 | Associar especialista ou pendência ao transbordo | Ethos | SPEC 1.4 | Produto sem especialista fica pendente visível | CA-1-011 / limite | captura da pendência | F1-T10 | ☐ |
| F1-T12 | Criar resolução/reabertura de transbordo | Ethos | SPEC 1.4 | Resolução devolve lead a status válido e preserva histórico | CA-1-012 / REGRESSÃO | captura/log da resolução | F1-T10 | ☐ |
| F1-T13 | Gerar oportunidade a partir de lead qualificado | Ethos | SPEC 1.5 | Lead qualificado cria oportunidade com justificativa | CA-1-013 / GREEN | captura da oportunidade | F1-T07 | ☐ |
| F1-T14 | Criar decisões aprovar/rejeitar/revisar | Ethos | SPEC 1.5 | Usuário registra decisão humana | CA-1-014 / GREEN | captura da decisão | F1-T13 | ☐ |
| F1-T15 | Exigir motivo em rejeição e bloquear duplicidade | Ethos | SPEC 1.5 | Rejeição sem motivo não salva; duplicidade sinalizada | CA-1-015 / erro+regressão | captura da validação | F1-T14 | ☐ |
| F1-T16 | Criar eventos/base de cálculo de métricas | Ethos | SPEC 1.6 | K1, K2 e K3 leem dados locais | CA-1-016, CA-1-017, CA-1-018 / GREEN | log/captura dos cálculos | F1-T07, F1-T19 | ☐ |
| F1-T17 | Criar painel mínimo com filtros e estado sem dados | Ethos | SPEC 1.6 | Painel mostra KPIs ou insuficiência sem erro | CA-1-016, CA-1-017, CA-1-018 / REGRESSÃO | captura do painel | F1-T16 | ☐ |
| F1-T18 | Criar fixture de baseline com 10 leads | Ethos | SPEC 1.6 | Fixture demonstra K1 70% e K2/K3 calculáveis | TDD GREEN SPEC 1.6 | captura do painel com fixture | F1-T17 | ☐ |
| F1-T19 | Criar timeline de interações no lead | Ethos | SPEC 1.7 | Interação humana com data/responsável é registrada | CA-1-019 / GREEN | captura da timeline | F1-T04 | ☐ |
| F1-T20 | Implementar marcação de proposta e contagem K2 | Ethos | SPEC 1.7 | Proposta fecha contagem de interações | CA-1-020 / GREEN | captura da contagem | F1-T19 | ☐ |
| F1-T21 | Implementar cancelamento auditável de interação | Ethos | SPEC 1.7 | Interação cancelada sai do K2 e preserva histórico | CA-1-021 / REGRESSÃO | captura/log do cancelamento | F1-T20 | ☐ |
