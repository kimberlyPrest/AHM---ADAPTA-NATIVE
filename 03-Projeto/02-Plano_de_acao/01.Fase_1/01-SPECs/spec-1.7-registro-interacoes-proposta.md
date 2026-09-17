# SPEC-1-007 — Registro de interações até proposta

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-04 / C-06  
**Degrau da solução:** construção mínima — K2 depende de contar esforço humano mesmo antes de integrações.

## Contexto e decisões fechadas

- **Estado atual:** esforço até proposta é citado como cerca de 8 interações, mas precisa de registro operacional.
- **Estado desejado:** usuário registra interações humanas por lead até a etapa de proposta para alimentar K2.
- **Decisões já fechadas:** Fase 1 não envia mensagens; registra eventos manuais.
- **Bloqueios:** nenhum.

## Resultado observável

Cada lead possui uma linha do tempo simples de interações humanas, com tipo, data, responsável, observação e marcação de proposta.

## Limites e dependências

- **Inclui:** registrar interação, listar histórico, contar interações humanas, marcar proposta enviada/atingida.
- **Fora de escopo:** envio de e-mail/WhatsApp, proposta comercial automática, integração calendário.
- **Entradas e pré-condições:** lead cadastrado.
- **Saídas/artefatos:** histórico de interações e evento de proposta.
- **Dependências e responsáveis:** comercial registra interações.
- **Atores e permissões mínimas:** usuário interno cria/interpreta; admin corrige se necessário.
- **Superfícies/arquivos/configurações afetadas:** detalhe do lead e métrica K2.
- **Risco e plano B:** usuário não registrar; painel deve expor ausência de dados.
- **Rollback ou reversão:** interação pode ser marcada como cancelada/corrigida preservando histórico.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Lead | Sistema Fase 1 | tipo, data, responsável, observação, conta_k2, proposta | usuário interno | evitar duplicidade por data+tipo+responsável opcional | validação de campos |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.20 | interação marcada humana | conta para K2 | interação interna de correção não conta | AHM.pdf |
| RN-1.21 | proposta marcada | encerra contagem principal até proposta | reabertura exige motivo | escopo definitivo |
| RN-1.22 | interação cancelada | preserva histórico e remove da contagem | auditoria mantém registro | governança |

## Fluxo e regras

1. Usuário abre lead.
2. Registra interação humana.
3. Sistema atualiza contagem K2.
4. Usuário marca proposta quando atingida/enviada.
5. Painel usa contagem até proposta.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | lead com 3 interações e proposta | K2 do lead = 3 | não aplicável |
| Limite | interação não conta K2 | aparece no histórico sem compor métrica | usuário identifica tipo |
| Falha | data futura inválida | impedir ou pedir confirmação | mensagem clara |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPEC 1.2 e 1.6.
2. **Alterar somente:** histórico de interações no lead e evento de proposta.
3. **Não alterar:** envio externo e automação de follow-up.
4. **Executar nesta ordem:** modelo, formulário, timeline, contagem, testes.
5. **Parar e pedir validação quando:** tipo de interação mudar definição do K2.
6. **Estado válido ao parar:** lead abre mesmo sem interações.

## Checklist de execução

- [ ] Interação pode ser registrada.
- [ ] Timeline aparece no lead.
- [ ] Interações humanas contam para K2.
- [ ] Proposta encerra contagem.
- [ ] Correção/cancelamento preserva histórico.

## Critérios de aceite

- [ ] **CA-1-019:** lead registra interação humana com data e responsável.
- [ ] **CA-1-020:** marcação de proposta fecha contagem do K2.
- [ ] **CA-1-021:** interação cancelada não conta para K2 e permanece auditável.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lead sem timeline | abrir lead | não há contagem de interações | captura/log |
| GREEN | registrar 3 interações e proposta | criar eventos no lead | K2 do lead = 3 | captura/log |
| REFACTOR/REGRESSÃO | cancelar uma interação | cancelar evento | contagem atualiza para 2 e histórico preserva cancelamento | captura/log |

**Dados/fixtures:** lead manual com responsável e três interações.  
**Caminhos de erro obrigatórios:** campos obrigatórios ausentes, data inválida, cancelamento.  
**Evidência exigida:** captura/log da timeline e contagem.

## Handoff e operação

- **Como demonstrar:** criar lead, registrar interações, marcar proposta e conferir K2.
- **Como operar depois:** comercial registra interações até a integração existir.
- **Como monitorar:** leads sem interação, leads com proposta e média K2.
- **Pendência conhecida:** captura automática de interações fica para fases futuras.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T19 | Criar timeline de interações no lead | Ethos | SPEC 1.7 | Interação humana com data/responsável é registrada | CA-1-019 | captura | F1-T04 | ☐ |
| F1-T20 | Implementar marcação de proposta e contagem K2 | Ethos | SPEC 1.7 | Proposta fecha contagem de interações | CA-1-020 | captura | F1-T19 | ☐ |
| F1-T21 | Implementar cancelamento auditável de interação | Ethos | SPEC 1.7 | Interação cancelada sai do K2 e preserva histórico | CA-1-021 | captura/log | F1-T20 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
