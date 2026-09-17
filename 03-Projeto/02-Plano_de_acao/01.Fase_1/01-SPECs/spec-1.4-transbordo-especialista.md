# SPEC-1-004 — Roteamento de transbordo para especialista

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-07 / C-04  
**Degrau da solução:** construção mínima — reduz risco de resposta automática indevida.

## Contexto e decisões fechadas

- **Estado atual:** perguntas técnicas dependem de especialistas; a reunião explicitou risco de alucinação quando produtos têm ecossistemas diferentes.
- **Estado desejado:** lead ou pergunta complexa gera transbordo com motivo, especialista e próximo passo.
- **Decisões já fechadas:** IA não força resposta técnica; transbordo é obrigatório quando base não sustenta resposta.
- **Bloqueios:** nenhum.

## Resultado observável

Casos complexos aparecem numa fila de transbordo com produto, motivo, especialista e status de tratamento.

## Limites e dependências

- **Inclui:** status de transbordo, motivo, especialista, comentário e resolução manual.
- **Fora de escopo:** notificação automática por e-mail/WhatsApp, agente especialista autônomo.
- **Entradas e pré-condições:** produto com especialista ou pendência de especialista.
- **Saídas/artefatos:** item de transbordo vinculado ao lead.
- **Dependências e responsáveis:** AHM valida especialistas por produto.
- **Atores e permissões mínimas:** comercial cria/revisa; especialista resolve.
- **Superfícies/arquivos/configurações afetadas:** fila de transbordos e lead.
- **Risco e plano B:** especialista ausente; item fica em pendência sem bloquear sistema.
- **Rollback ou reversão:** transbordo pode ser reaberto com motivo.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Lead/produto | Sistema Fase 1 | lead, produto, motivo, especialista, status, comentário, resolução | usuário interno | não aplicável | especialista ausente vira pendência |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.11 | regra detecta dúvida técnica | criar transbordo | se não houver especialista, marcar pendente | reunião final |
| RN-1.12 | especialista responde | registrar resolução e devolver lead à qualificação ou oportunidade | se resposta insuficiente, manter aberto | escopo definitivo |

## Fluxo e regras

1. Motor ou usuário marca transbordo.
2. Sistema registra motivo e especialista.
3. Especialista adiciona resolução.
4. Lead volta para qualificação/oportunidade ou permanece pendente.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | produto tem especialista | item criado atribuído | não aplicável |
| Limite | produto sem especialista | item pendente de atribuição | usuário define especialista |
| Falha | resolução vazia | impedir fechamento | pedir comentário |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPEC 1.1 e 1.3.
2. **Alterar somente:** fila/status de transbordo.
3. **Não alterar:** envio externo ou agentes autônomos.
4. **Executar nesta ordem:** modelo, criação automática/manual, fila, resolução, testes.
5. **Parar e pedir validação quando:** transbordo exigir alçada comercial não prevista.
6. **Estado válido ao parar:** leads com transbordo ficam acessíveis e não somem da fila.

## Checklist de execução

- [ ] Item de transbordo pode ser criado.
- [ ] Motivo é obrigatório.
- [ ] Especialista é sugerido por produto quando existir.
- [ ] Fechamento exige resolução.
- [ ] Lead preserva histórico.

## Critérios de aceite

- [ ] **CA-1-010:** lead complexo cria transbordo com motivo.
- [ ] **CA-1-011:** transbordo sem especialista fica pendente visível.
- [ ] **CA-1-012:** resolução devolve lead para próximo status válido.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lead complexo sem fila | qualificar lead técnico | não há tratamento estruturado | captura/log |
| GREEN | criar transbordo | marcar motivo técnico | item aparece na fila | captura/log |
| REFACTOR/REGRESSÃO | resolver e reabrir | fechar com comentário e reabrir | histórico preservado | captura/log |

**Dados/fixtures:** lead técnico, produto com especialista, produto sem especialista.  
**Caminhos de erro obrigatórios:** motivo vazio, especialista ausente, resolução vazia.  
**Evidência exigida:** captura/log da fila e resolução.

## Handoff e operação

- **Como demonstrar:** qualificar lead técnico e resolver transbordo.
- **Como operar depois:** especialista revisa fila em cadência combinada.
- **Como monitorar:** quantidade e idade dos transbordos.
- **Pendência conhecida:** automação de notificação fica para fase futura.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T10 | Criar fila/modelo de transbordo | Ethos | SPEC 1.4 | Lead complexo cria item com motivo | CA-1-010 | captura | F1-T07 | ☐ |
| F1-T11 | Associar especialista ou pendência ao transbordo | Ethos | SPEC 1.4 | Produto sem especialista fica pendente visível | CA-1-011 | captura | F1-T10 | ☐ |
| F1-T12 | Criar resolução/reabertura de transbordo | Ethos | SPEC 1.4 | Resolução devolve lead a status válido e preserva histórico | CA-1-012 | captura/log | F1-T10 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
