# SPEC-1-005 — Fila de oportunidades e decisão humana

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-08 / C-05  
**Degrau da solução:** construção mínima — permite validar valor antes de automação de contato.

## Contexto e decisões fechadas

- **Estado atual:** oportunidades dependem de análise humana e histórico disperso.
- **Estado desejado:** leads qualificados geram oportunidades explicáveis para aprovação, rejeição ou revisão.
- **Decisões já fechadas:** agente sugere antes de automatizar contato; contato automático está fora da Fase 1.
- **Bloqueios:** nenhum.

## Resultado observável

Uma fila exibe oportunidades geradas a partir de leads qualificados, com produto, justificativa, confiança simples e decisão humana.

## Limites e dependências

- **Inclui:** geração de oportunidade, fila, decisão `aprovar/rejeitar/revisar`, comentário e histórico.
- **Fora de escopo:** envio de mensagens, proposta formal automática, precificação.
- **Entradas e pré-condições:** lead qualificado e produto ativo.
- **Saídas/artefatos:** oportunidade com status e decisão.
- **Dependências e responsáveis:** comercial valida decisões.
- **Atores e permissões mínimas:** comercial decide; consultora/admin configura.
- **Superfícies/arquivos/configurações afetadas:** fila de oportunidades.
- **Risco e plano B:** sugestão ruim; rejeição com motivo alimenta ajuste futuro.
- **Rollback ou reversão:** oportunidade aprovada pode voltar para revisão com justificativa.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Lead qualificado | Sistema Fase 1 | lead, produto, motivo, confiança, status, decisão, comentário | usuário interno | uma oportunidade ativa por lead/produto | duplicidade sinalizada |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.13 | lead qualificado | criar oportunidade sugerida | se já existe oportunidade aberta, atualizar justificativa | escopo definitivo |
| RN-1.14 | usuário rejeita | exigir motivo | motivo opcional só para revisão | decisão de melhoria |
| RN-1.15 | oportunidade aprovada | marcar pronta para próxima ação humana | não enviar contato automático | reunião final |

## Fluxo e regras

1. Lead qualificado cria/atualiza oportunidade.
2. Usuário abre fila e vê justificativa.
3. Usuário aprova, rejeita ou pede revisão.
4. Decisão fica registrada para métricas e melhoria.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | lead qualificado | oportunidade sugerida | não aplicável |
| Limite | oportunidade duplicada | atualizar existente ou bloquear duplicata | mostrar aviso |
| Falha | rejeição sem motivo | impedir salvar rejeição | pedir motivo |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPEC 1.3.
2. **Alterar somente:** oportunidade e decisão humana.
3. **Não alterar:** comunicação externa ou proposta automática.
4. **Executar nesta ordem:** criação, fila, decisões, histórico, testes.
5. **Parar e pedir validação quando:** decisão humana exigir nova categoria.
6. **Estado válido ao parar:** oportunidades existentes continuam visíveis.

## Checklist de execução

- [ ] Oportunidade surge a partir de lead qualificado.
- [ ] Justificativa é exibida.
- [ ] Aprovar/rejeitar/revisar funcionam.
- [ ] Rejeição exige motivo.
- [ ] Não há envio externo.

## Critérios de aceite

- [ ] **CA-1-013:** lead qualificado gera oportunidade.
- [ ] **CA-1-014:** usuário registra decisão humana.
- [ ] **CA-1-015:** rejeição sem motivo é bloqueada.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lead qualificado sem fila | qualificar lead | não existe decisão de oportunidade | captura/log |
| GREEN | gerar e aprovar | qualificar lead e aprovar oportunidade | status aprovado registrado | captura/log |
| REFACTOR/REGRESSÃO | rejeitar sem motivo | tentar rejeitar vazio | validação bloqueia | captura/log |

**Dados/fixtures:** lead qualificado e produto ativo.  
**Caminhos de erro obrigatórios:** duplicidade, rejeição sem motivo, lead não qualificado.  
**Evidência exigida:** captura/log da fila e decisão.

## Handoff e operação

- **Como demonstrar:** gerar oportunidade para lead fixture e registrar três decisões.
- **Como operar depois:** comercial revisa fila diariamente.
- **Como monitorar:** aprovadas, rejeitadas, revisão e motivos.
- **Pendência conhecida:** contato automático fica fora da Fase 1.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T13 | Gerar oportunidade a partir de lead qualificado | Ethos | SPEC 1.5 | Lead qualificado cria oportunidade com justificativa | CA-1-013 | captura | F1-T07 | ☐ |
| F1-T14 | Criar decisões aprovar/rejeitar/revisar | Ethos | SPEC 1.5 | Usuário registra decisão humana | CA-1-014 | captura | F1-T13 | ☐ |
| F1-T15 | Exigir motivo em rejeição e bloquear duplicidade | Ethos | SPEC 1.5 | Rejeição sem motivo não salva; duplicidade sinalizada | CA-1-015 | captura | F1-T14 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
