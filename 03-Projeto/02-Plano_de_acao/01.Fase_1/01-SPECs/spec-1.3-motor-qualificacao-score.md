# SPEC-1-003 — Motor de qualificação e score explicável

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-03 / C-03  
**Degrau da solução:** construção mínima — qualificação precisa ser auditável antes de qualquer IA ou integração.

## Contexto e decisões fechadas

- **Estado atual:** a qualificação depende de conhecimento humano e conversas; o briefing mede K1 como qualificação sem intervenção humana.
- **Estado desejado:** sistema classifica leads manuais/importados com regras transparentes e justificativa.
- **Decisões já fechadas:** a Fase 1 usa regra local, não API; saídas são `qualificado`, `pendente`, `fora do recorte` e `transbordo`.
- **Bloqueios:** nenhum.

## Resultado observável

Um lead pode ser qualificado a partir de dados manuais e produto selecionado, recebendo status, score/resultado e motivos legíveis.

## Limites e dependências

- **Inclui:** motor de regras, justificativa, reprocessamento após edição, marcação de transbordo.
- **Fora de escopo:** modelo generativo autônomo, contato automático, recomendação avançada de recompra.
- **Entradas e pré-condições:** produto ativo e lead cadastrado.
- **Saídas/artefatos:** status de qualificação, razões, pendências e timestamp.
- **Dependências e responsáveis:** regras iniciais vêm do escopo e catálogo.
- **Atores e permissões mínimas:** comercial pode reprocessar; admin pode ajustar regra se a interface existir.
- **Superfícies/arquivos/configurações afetadas:** módulo de qualificação.
- **Risco e plano B:** regra simplificada classificar demais; plano B é pendência ou transbordo, nunca resposta forçada.
- **Rollback ou reversão:** permitir voltar lead para `pendente` e registrar motivo.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Lead + produto | Sistema Fase 1 | produto, dor, urgência, origem, respostas, complexidade, critérios | usuário interno | reprocessamento idempotente com mesmas entradas | erro vira pendência com mensagem |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.7 | lead tem produto ativo, dor compatível e perguntas mínimas respondidas | classificar como `qualificado` | se houver sinal técnico complexo, `transbordo` | escopo definitivo |
| RN-1.8 | faltam respostas obrigatórias | classificar como `pendente` | não contar como qualificado K1 | escopo definitivo |
| RN-1.9 | produto fora do recorte piloto | classificar como `fora do recorte` | manter visível para análise | escopo definitivo |
| RN-1.10 | dúvida técnica ou risco | classificar como `transbordo` | exige especialista | reunião final |

## Fluxo e regras

1. Usuário abre lead.
2. Seleciona/confirmar produto de interesse e respostas.
3. Sistema executa regras.
4. Sistema grava status e justificativa.
5. Métricas K1 recebem evento de qualificação quando aplicável.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | dados completos e baixa complexidade | `qualificado` com motivos | não aplicável |
| Limite | falta uma pergunta obrigatória | `pendente` com pendência | usuário completa e reprocessa |
| Falha | produto inexistente/inativo | qualificação bloqueada | selecionar produto ativo |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPEC 1.1 e 1.2.
2. **Alterar somente:** regra de qualificação local e persistência do resultado.
3. **Não alterar:** integrações, IA generativa, envio externo.
4. **Executar nesta ordem:** regras, estados, justificativa, eventos de métrica, testes.
5. **Parar e pedir validação quando:** regra exigir decisão comercial nova.
6. **Estado válido ao parar:** leads existentes continuam listáveis.

## Checklist de execução

- [ ] Estados de qualificação implementados.
- [ ] Justificativa aparece para cada resultado.
- [ ] Reprocessamento após edição funciona.
- [ ] Pendente/transbordo não conta como qualificação automática.
- [ ] Erros não apagam dados do lead.

## Critérios de aceite

- [ ] **CA-1-007:** lead completo de baixa complexidade vira `qualificado` com justificativa.
- [ ] **CA-1-008:** lead incompleto vira `pendente` com pendências.
- [ ] **CA-1-009:** lead técnico/complexo vira `transbordo`.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lead não qualifica antes do motor | abrir lead completo | sem classificação automática | captura/log |
| GREEN | classificar baixa complexidade | executar qualificação | `qualificado` + motivos | captura/log |
| REFACTOR/REGRESSÃO | editar lead para complexo | marcar sinal técnico | resultado muda para `transbordo` sem perder histórico | captura/log |

**Dados/fixtures:** lead completo, lead incompleto, lead técnico.  
**Caminhos de erro obrigatórios:** produto inativo, campos vazios, regra sem correspondência.  
**Evidência exigida:** captura/log do status e justificativa.

## Handoff e operação

- **Como demonstrar:** qualificar três leads fixture cobrindo qualificado, pendente e transbordo.
- **Como operar depois:** comercial revisa exceções e rejeições.
- **Como monitorar:** distribuição de status e taxa de pendências.
- **Pendência conhecida:** regras podem ser calibradas após uso real.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T07 | Implementar estados e regras iniciais de qualificação | Ethos | SPEC 1.3 | Lead completo vira qualificado; incompleto vira pendente | CA-1-007, CA-1-008 | captura/log | F1-T01, F1-T04 | ☐ |
| F1-T08 | Implementar justificativa e reprocessamento de qualificação | Ethos | SPEC 1.3 | Resultado mostra motivos e muda após edição | CA-1-007, CA-1-009 | captura/log | F1-T07 | ☐ |
| F1-T09 | Implementar tratamento de produto inativo/inexistente na qualificação | Ethos | SPEC 1.3 | Qualificação bloqueia produto inválido sem apagar lead | CA-1-009 | captura/log | F1-T07 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
