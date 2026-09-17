# SPEC-1-001 — Cadastro e manutenção do catálogo de produtos

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-06 / C-01  
**Degrau da solução:** construção mínima — precisa existir dentro do sistema para sustentar qualificação sem integrações.

## Contexto e decisões fechadas

- **Estado atual:** a AHM possui conhecimento por produto distribuído em vídeos, documentos e especialistas; a última reunião confirmou que cada produto precisa ter ICP, escopo, perguntas, FAQ, timing e especialista.
- **Estado desejado:** usuário cadastra e consulta produtos/famílias com informações suficientes para qualificação inicial e transbordo.
- **Decisões já fechadas:** Fase 1 não depende de API; produto pode ser criado manualmente; venda recorrente e pontual podem coexistir se houver regras por produto.
- **Bloqueios:** nenhum para construir a estrutura; conteúdo real pode ser alimentado progressivamente.

## Resultado observável

Uma tela ou módulo permite criar, editar, listar e consultar produtos/famílias com ICP, dor, escopo, perguntas, timing, critérios de complexidade e especialista responsável.

## Limites e dependências

- **Inclui:** CRUD/manual de produtos, campos obrigatórios, status ativo/inativo, especialista e critérios de baixa/alta complexidade.
- **Fora de escopo:** importação automática de catálogo, integração com ERP, precificação e estoque.
- **Entradas e pré-condições:** usuário interno autenticado no ambiente do sistema; lista inicial pode ser fictícia ou amostra validada.
- **Saídas/artefatos:** registros de produto disponíveis para o motor de qualificação.
- **Dependências e responsáveis:** consultora define campos; AHM valida conteúdo.
- **Atores e permissões mínimas:** admin/consultora pode criar e editar; comercial pode consultar.
- **Superfícies/arquivos/configurações afetadas:** módulo de catálogo e armazenamento local do app.
- **Risco e plano B:** se conteúdo real não chegar, usar produto exemplo marcado como amostra.
- **Rollback ou reversão:** produto pode ser inativado sem apagar histórico.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Cadastro manual | Sistema da Fase 1 | nome, família, tipo de venda, ICP, dor, escopo, FAQs, perguntas, timing, complexidade, especialista, status | usuário interno | não aplicável | validação de campos obrigatórios |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.1 | produto sem nome, ICP ou perguntas | impedir salvar | rascunho permitido somente se marcado incompleto | escopo definitivo |
| RN-1.2 | produto com pergunta técnica fora do básico | exigir especialista | se não houver especialista, classificar como pendência | reunião final |
| RN-1.3 | produto inativo | não usar em qualificação nova | manter visível em histórico | escopo definitivo |

## Fluxo e regras

1. Usuário acessa catálogo.
2. Cria produto/família com campos obrigatórios.
3. Define perguntas de qualificação e critérios de baixa/alta complexidade.
4. Define especialista responsável.
5. Produto ativo fica disponível para qualificação.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | produto completo | produto salvo e disponível | não aplicável |
| Limite | produto sem especialista | salvar como incompleto ou impedir ativação | pendência visível |
| Falha | nome duplicado na mesma família | bloquear duplicidade ou pedir confirmação | manter registro original |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** escopo definitivo seções 6 e Fase 1; matriz R-06.
2. **Alterar somente:** estrutura de dados e interface do catálogo.
3. **Não alterar:** integrações, CRM, RD, Envia, Omie.
4. **Executar nesta ordem:** modelo de dados, validações, tela/lista, estados vazio/erro, testes.
5. **Parar e pedir validação quando:** surgir campo comercial não previsto que altere qualificação.
6. **Estado válido ao parar:** catálogo abre, lista produtos e não quebra qualificação.

## Checklist de execução

- [ ] Campos obrigatórios criados.
- [ ] Produto pode ser criado, editado, consultado e inativado.
- [ ] Estado vazio e erro de validação existem.
- [ ] Especialista pode ser associado.
- [ ] Evidência de produto exemplo registrada.

## Critérios de aceite

- [ ] **CA-1-001:** produto completo pode ser salvo e aparece na lista.
- [ ] **CA-1-002:** produto sem campos obrigatórios não fica ativo para qualificação.
- [ ] **CA-1-003:** produto inativo não aparece como opção em novo lead.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | tentar qualificar lead sem catálogo | abrir qualificação com catálogo vazio | sistema informa que não há produto ativo | captura/log |
| GREEN | criar produto completo | preencher e salvar produto exemplo | produto aparece ativo na lista | captura/log |
| REFACTOR/REGRESSÃO | editar/inativar produto | alterar timing e inativar | histórico preservado e produto some da seleção nova | captura/log |

**Dados/fixtures:** produto exemplo `Shockwatch recorrente baixa complexidade`, ICP B2B industrial, especialista `Especialista Produto`.  
**Caminhos de erro obrigatórios:** vazio, duplicado, obrigatório ausente.  
**Evidência exigida:** captura ou log do produto ativo e da validação de erro.

## Handoff e operação

- **Como demonstrar:** cadastrar um produto exemplo e usá-lo como opção em lead manual.
- **Como operar depois:** AHM mantém produto e especialista atualizados.
- **Como monitorar:** produtos incompletos aparecem em pendências.
- **Pendência conhecida:** conteúdo real de produtos pode substituir fixtures.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T01 | Criar modelo e validações do catálogo de produtos | Ethos | SPEC 1.1 | Campos obrigatórios impedem produto incompleto ativo | CA-1-001, CA-1-002 | log/captura | nenhuma | ☐ |
| F1-T02 | Criar tela/lista de catálogo com ativar/inativar | Ethos | SPEC 1.1 | Produto ativo aparece; inativo some de nova seleção | CA-1-001, CA-1-003 | captura | F1-T01 | ☐ |
| F1-T03 | Criar fixture de produto exemplo para demonstração | Ethos | SPEC 1.1 | Produto exemplo sustenta qualificação manual | CA-1-001 | captura | F1-T02 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
