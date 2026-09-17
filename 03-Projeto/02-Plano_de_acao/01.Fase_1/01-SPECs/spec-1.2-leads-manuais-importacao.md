# SPEC-1-002 — Cadastro e importação manual de leads

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-02 / C-02  
**Degrau da solução:** construção mínima — substitui temporariamente integrações para provar valor na Fase 1.

## Contexto e decisões fechadas

- **Estado atual:** leads chegam por canais diversos, mas a Fase 1 não pode depender de RD, Pipedrive, Envia ou API.
- **Estado desejado:** usuário cria lead manualmente ou importa uma lista simples para alimentar qualificação.
- **Decisões já fechadas:** integração fica para Fase 2; Fase 1 aceita entrada manual, CSV/planilha ou amostra validada.
- **Bloqueios:** nenhum.

## Resultado observável

Leads podem ser cadastrados manualmente ou importados por arquivo estruturado, com campos mínimos para qualificação e status inicial.

## Limites e dependências

- **Inclui:** criação manual, listagem, edição, importação CSV/planilha simples, validação de campos e deduplicação local básica.
- **Fora de escopo:** sincronização CRM, envio WhatsApp/e-mail, enriquecimento automático.
- **Entradas e pré-condições:** catálogo de produtos pode estar vazio, mas qualificação exige produto ativo.
- **Saídas/artefatos:** lead com status `novo`, `em qualificação`, `qualificado`, `pendente`, `fora do recorte` ou `transbordo`.
- **Dependências e responsáveis:** AHM fornece amostra quando houver; sistema deve aceitar fixture.
- **Atores e permissões mínimas:** usuário interno pode criar/importar; admin pode excluir/inativar.
- **Superfícies/arquivos/configurações afetadas:** módulo de leads e armazenamento local do app.
- **Risco e plano B:** arquivo fora do padrão; plano B é cadastro manual.
- **Rollback ou reversão:** importação deve permitir cancelar antes de confirmar.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Cadastro manual/importação | Sistema da Fase 1 | nome, empresa, contato, origem, produto de interesse, dor, urgência, observações, responsável opcional | usuário interno | importação idempotente por contato+empresa quando possível | relatório de linhas rejeitadas |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.4 | lead sem nome ou empresa/contato | impedir confirmação | rascunho permitido se marcado incompleto | escopo definitivo |
| RN-1.5 | contato+empresa já existe | sinalizar possível duplicidade | permitir criar se usuário confirmar | escopo definitivo |
| RN-1.6 | produto de interesse ausente | manter como pendente | qualificação pode pedir seleção posterior | Fase 1 |

## Fluxo e regras

1. Usuário escolhe cadastrar lead ou importar arquivo.
2. Sistema valida campos mínimos.
3. Sistema mostra duplicidades e erros.
4. Usuário confirma criação/importação.
5. Leads entram na fila com status inicial.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | lead completo | lead criado como `novo` | não aplicável |
| Limite | CSV com linhas válidas e inválidas | válidas importadas após confirmação; inválidas listadas | baixar/visualizar erros |
| Falha | arquivo ilegível | nada importado | mensagem clara e retorno ao cadastro manual |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPEC 1.1 e escopo Fase 1.
2. **Alterar somente:** módulo de leads/importação.
3. **Não alterar:** conectores, APIs, envio automático.
4. **Executar nesta ordem:** campos, cadastro manual, listagem, importação, validação, testes.
5. **Parar e pedir validação quando:** surgir necessidade de campo que mude K1/K2/K3.
6. **Estado válido ao parar:** leads manuais continuam acessíveis.

## Checklist de execução

- [ ] Cadastro manual salva lead válido.
- [ ] Importação mostra prévia antes de confirmar.
- [ ] Linhas inválidas são reportadas.
- [ ] Possível duplicidade é sinalizada.
- [ ] Lead criado aparece na fila.

## Critérios de aceite

- [ ] **CA-1-004:** lead manual completo pode ser criado.
- [ ] **CA-1-005:** importação parcial informa linhas rejeitadas sem perder válidas.
- [ ] **CA-1-006:** duplicidade local é sinalizada antes de confirmar.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lista sem entrada manual | tentar criar lead antes do módulo existir | ação indisponível/falha controlada | captura/log |
| GREEN | criar e listar lead | cadastrar lead completo | lead aparece como `novo` | captura/log |
| REFACTOR/REGRESSÃO | importar CSV misto | importar 2 válidos e 1 inválido | sistema importa válidos e relata inválido | captura/log |

**Dados/fixtures:** CSV com `nome,empresa,contato,origem,produto_interesse,dor,urgencia`.  
**Caminhos de erro obrigatórios:** campo obrigatório ausente, duplicado, arquivo inválido.  
**Evidência exigida:** captura/log da criação e relatório de importação.

## Handoff e operação

- **Como demonstrar:** cadastrar 1 lead manual e importar 3 leads de teste.
- **Como operar depois:** Rose/comercial alimenta leads até integrações da Fase 2.
- **Como monitorar:** quantidade de leads novos e importações com erro.
- **Pendência conhecida:** integração automática virá na Fase 2.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T04 | Criar modelo e formulário de lead manual | Ethos | SPEC 1.2 | Lead completo pode ser criado como novo | CA-1-004 | captura/log | nenhuma | ☐ |
| F1-T05 | Criar importação CSV com prévia e relatório de erros | Ethos | SPEC 1.2 | Válidos importam e inválidos são reportados | CA-1-005 | relatório | F1-T04 | ☐ |
| F1-T06 | Criar sinalização de duplicidade local | Ethos | SPEC 1.2 | Duplicidade contato+empresa é avisada antes de confirmar | CA-1-006 | captura | F1-T04 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
