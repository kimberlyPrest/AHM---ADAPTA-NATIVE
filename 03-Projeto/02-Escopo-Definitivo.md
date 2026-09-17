# Escopo Definitivo — AHM Solution do Brasil

**Data:** 2026-09-17  
**Status:** aprovado por cliente, CSM e consultora para seguir para SPECs e tasks  
**Run:** `20260917T124459723Z-8a62ef32`

## 1. Resultado de negócio

Automatizar a qualificação e o acompanhamento comercial dos produtos recorrentes e de baixa complexidade da AHM, aumentando a capacidade do time comercial sem ampliar a equipe na mesma proporção.

O escopo definitivo foca o trecho de maior impacto acordado na última reunião: entrada do lead, qualificação, recomendação de produto, follow-up, proposta e oportunidades de recompra/expansão. Integrações com Pipedrive, RD Station e Envia entram como evolução a partir da Fase 2. A Fase 1 não depende de APIs, credenciais, webhooks, conectores externos ou qualquer bloqueador técnico do cliente.

## 2. Critérios globais de sucesso

| Critério | Definição | Meta |
|---|---|---|
| K1 — Qualificação inicial automatizada | leads do recorte piloto qualificados sem intervenção humana / leads recebidos no recorte piloto | >= 70% |
| K2 — Redução de esforço humano até proposta | média de interações humanas entre entrada do lead e proposta | reduzir >= 50%; referência inicial: cerca de 8 para <= 4 |
| K3 — Capacidade por vendedor | leads processados por vendedor após a solução / baseline | >= 1,30x |

Na Fase 1, quando não houver dados integrados, a medição pode usar entrada manual, planilha, importação CSV ou amostra operacional validada pela AHM. A integração automatizada da coleta não é requisito para provar o modelo.

## 3. Atores

| Ator | Papel |
|---|---|
| Afonso | dono comercial, valida produtos, ICPs, regras e oportunidades |
| Rose | referência operacional/financeira, apoia histórico e rotina de uso |
| Mário | sócio e visão estratégica/internacional |
| Pré-vendas / Comercial | usa a fila, aprova oportunidades e atua nos casos de transbordo |
| Especialistas de produto | validam regras técnicas por família de produto |
| Bart / Ethos | operador interno do projeto para skills, automações, execução assistida e loops |

## 4. Decisões consolidadas

| ID | Decisão aprovada | Efeito |
|---|---|---|
| D-01 | O projeto está aprovado por cliente, CSM e consultora. | Pode seguir para SPECs e tasks sem novo gate de aprovação de escopo. |
| D-02 | A Fase 1 não dependerá de integrações, APIs, credenciais ou bloqueadores externos. | Fase 1 será composta por SPECs/tasks executáveis com dados manuais, arquivos, amostras ou cadastros internos. |
| D-03 | O foco inicial é qualificação comercial, recomendação e avanço até proposta. | ERP, fiscal, faturamento e Omie ficam fora do MVP. |
| D-04 | Venda recorrente e venda pontual podem coexistir, desde que cada produto tenha ICP, descrição, perguntas, timing e regra. | Base de conhecimento por produto é requisito central. |
| D-05 | Perguntas técnicas devem transbordar para humano ou especialista, em vez de forçar resposta automática. | O sistema precisa ter regra de segurança e roteamento por complexidade. |
| D-06 | O agente começa sugerindo oportunidades para aprovação humana antes de qualquer contato automático. | A autonomia comercial cresce por validação, não por suposição. |
| D-07 | Pipedrive, RD Station e Envia são integrações desejadas, mas não bloqueiam a Fase 1. | Elas entram em Fase 2, com fallback manual quando necessário. |
| D-08 | Bart/Ethos será usado como operador do projeto e poderá executar rotinas, skills e automações autorizadas. | Fases 4 e 5 acrescentam loops/agentes. |

## 5. Fluxo alvo

1. Lead entra por canal comercial ou é cadastrado/importado manualmente.
2. Sistema classifica o lead pelo recorte piloto, produto provável, maturidade, urgência e complexidade.
3. Motor de qualificação indica `qualificado`, `pendente`, `fora do recorte` ou `transbordo`.
4. Quando houver dúvida técnica ou risco, o caso vai para especialista.
5. Oportunidades de venda, recompra e expansão aparecem em fila para aprovação humana.
6. Follow-up e proposta são acompanhados com contagem de interações humanas.
7. Na evolução, integrações automatizam entrada, atualização de pipeline e captura de histórico.
8. No Ethos, loops operam relatórios, pesquisas, execução assistida e melhoria contínua.

## 6. Capacidades

| ID | Capacidade | Descrição |
|---|---|---|
| C-01 | Base de conhecimento por produto | ICP, dor, escopo, FAQs, perguntas, critérios de baixa/alta complexidade, timing e especialista. |
| C-02 | Cadastro/importação manual de leads | Permite operar sem API na Fase 1 por formulário interno, tabela ou importação estruturada. |
| C-03 | Motor de qualificação inicial | Regras auditáveis para classificar lead e explicar o motivo. |
| C-04 | Roteamento de transbordo | Encaminha dúvida técnica ou caso incerto para especialista. |
| C-05 | Fila de oportunidades | Mostra recomendações com justificativa para aprovação/rejeição. |
| C-06 | Medição K1/K2/K3 | Registra baseline, interações, status e resultado até proposta. |
| C-07 | Integrações comerciais | RD Station, Pipedrive e Envia, a partir da Fase 2. |
| C-08 | Loops e agentes Ethos | Bart opera relatórios, pesquisas, automações e execução assistida. |

## 7. Fora de escopo

- Omie, fiscal, tributação, DANFE, compra de suprimentos e faturamento.
- Integração completa CRM <-> ERP.
- Contato comercial totalmente autônomo sem aprovação humana prévia.
- Substituição de especialista técnico em decisão de aplicação complexa.
- Migração completa de histórico documental antigo.
- Mudança corporativa de Microsoft para Google Workspace.

## 8. Fases definitivas

### Fase 1 — Núcleo sem integrações: qualificação, catálogo e fila manual

**Princípio da fase:** nada desta fase pode depender de API, credencial, webhook, conector, acesso externo ou bloqueador do cliente. Se um dado ainda não estiver disponível por integração, ele deve poder ser cadastrado manualmente, importado por CSV/planilha ou simulado por amostra validada.

**Resultado mensurável:** AHM consegue qualificar leads do recorte piloto, medir K1/K2/K3 e revisar oportunidades usando uma operação interna mínima.

**Entrega visível:** sistema/tela operacional com cadastro ou importação de leads, catálogo de produtos, motor de qualificação, fila de oportunidades, transbordo e painel simples de métricas.

**Capacidades:** C-01, C-02, C-03, C-04, C-05 e C-06 em versão inicial.

**Atores:** Afonso, Rose, Comercial, especialistas de produto e consultora.

**Dados:** produtos, ICPs, FAQs, perguntas de qualificação, timing de venda, amostras de leads, histórico manual de interações e decisões humanas.

**Integrações:** nenhuma obrigatória.

**Regras:** lead pode ser criado manualmente; produto pode ser cadastrado pelo usuário; qualificação precisa explicar motivo; transbordo precisa registrar razão; oportunidade não dispara contato automático.

**SPECs sugeridas para a Fase 1:**
- SPEC 1.1 — Cadastro e manutenção do catálogo de produtos.
- SPEC 1.2 — Cadastro/importação manual de leads.
- SPEC 1.3 — Motor de qualificação e score explicável.
- SPEC 1.4 — Roteamento de transbordo para especialista.
- SPEC 1.5 — Fila de oportunidades e decisão humana.
- SPEC 1.6 — Painel mínimo de K1/K2/K3 e baseline.
- SPEC 1.7 — Registro de interações até proposta.

**Tasks esperadas:** cada SPEC deve ser quebrada em tasks pequenas, independentes e demonstráveis, permitindo executar e testar valor sem esperar qualquer integração.

**Checklist de aceite:**
- usuário cadastra produto/família com ICP, perguntas, timing e especialista;
- usuário cadastra ou importa lead manualmente;
- sistema classifica lead como qualificado, pendente, fora do recorte ou transbordo;
- classificação mostra justificativa;
- fila exibe oportunidades e permite aprovar, rejeitar ou pedir revisão;
- painel mostra baseline inicial de K1/K2/K3;
- nenhuma task da Fase 1 exige API, chave, webhook ou conector externo.

### Fase 2 — Integração comercial controlada

**Resultado mensurável:** entradas e atualizações comerciais passam a ser automatizadas quando APIs estiverem validadas.

**Entrega visível:** lead vindo de RD/Envia/Pipedrive cria ou atualiza registro do sistema sem duplicidade crítica.

**Capacidades:** C-07 conectada às capacidades da Fase 1.

**Integrações:** Pipedrive, RD Station e Envia.

**Regras:** integração nunca substitui o fluxo manual; falha de API gera pendência visível; dados externos passam pelas mesmas regras da Fase 1.

**Checklist de aceite:**
- pelo menos um fluxo de entrada integrado validado;
- falha de conexão tratada sem perda de lead;
- duplicidade sinalizada;
- pipeline/status atualizado conforme regra aprovada.

### Fase 3 — Recomendação, proposta e recompra assistida

**Resultado mensurável:** equipe recebe recomendações explicáveis de produto, proposta, recompra e expansão.

**Entrega visível:** fila de oportunidades enriquecida com histórico e recomendação comercial.

**Capacidades:** C-05 e C-06 aprofundadas.

**Dados:** histórico de clientes, compras, faturamentos, produtos atuais, timing e decisões de aprovação/rejeição.

**Regras:** recomendação deve citar evidência; contato automático permanece bloqueado sem aprovação; rejeições alimentam melhoria.

**Checklist de aceite:**
- oportunidade mostra cliente, produto, razão, fonte e confiança;
- usuário aprova/rejeita/ajusta;
- sistema registra aprendizado operacional;
- recompra usa timing validado por produto.

### Fase 4 — Operação assistida no Ethos e loops iniciais

**Resultado mensurável:** Bart/Ethos opera rotinas internas com cadência, limite de autonomia e veredito humano.

**Entrega visível:** skills e automações para relatório comercial, pesquisa de mercado/acidentes e execução assistida de tasks no Skip.

**Capacidades:** C-08, sem substituir o sistema entregue nas fases anteriores.

**Loops candidatos:**
- L-01 — Relatório comercial diário.
- L-02 — Pesquisa de acidentes envolvendo veículos industriais e pessoas.
- L-03 — Execução assistida de tasks no Skip.

**Checklist de aceite:**
- cada loop tem meta, cadência, fonte, destinatário e limite;
- falha gera pendência visível;
- humano confirma utilidade antes de manter ativo.

### Fase 5 — Automação controlada e validação ponta a ponta

**Resultado mensurável:** fases 1-5 validadas em conjunto, com decisão de go-live, ajustes ou rollback.

**Entrega visível:** fluxo piloto executado desde cadastro/entrada do lead até qualificação, oportunidade, proposta/recompra e relatório final.

**Capacidades:** C-01 a C-08 consolidadas.

**Matriz de validação:**

| Fase | O que provar | Evidência |
|---|---|---|
| Fase 1 | núcleo funciona sem integrações | lead manual qualificado, oportunidade gerada e métricas visíveis |
| Fase 2 | integrações alimentam o mesmo núcleo | log de entrada/atualização e falha tratada |
| Fase 3 | recomendações são úteis e explicáveis | decisões de aprovação/rejeição registradas |
| Fase 4 | loops têm cadência e utilidade | relatório de ciclos e veredito humano |
| Fase 5 | conjunto sustenta go-live assistido | ata de validação com K1/K2/K3 |

**Loops/agentes adicionais:** amadurecer loops úteis, avaliar agente especialista por família de produto e liberar automação de contato apenas em cenário validado.

**Checklist de aceite:**
- teste ponta a ponta executado;
- K1/K2/K3 calculados com fonte rastreável;
- limites de autonomia documentados;
- decisão final registrada.

## 9. Gates restantes

Como o projeto foi aprovado, os itens abaixo não bloqueiam a existência do escopo. Eles bloqueiam apenas a execução da parte afetada.

| Gate | Bloqueia |
|---|---|
| Entrega do catálogo inicial de produtos | SPECs de produto completas, mas não a estrutura da Fase 1 |
| APIs Pipedrive/RD/Envia | Fase 2, não Fase 1 |
| Histórico de compras/faturamento | Recomendações avançadas da Fase 3 |
| Canais/conectores do Bart | Loops da Fase 4 |

## 10. Matriz fonte -> decisão -> requisito -> fase

| Fonte / achado | Decisão | Requisito | Fase |
|---|---|---|---|
| Aprovação informada pela consultora | seguir para SPECs/tasks | escopo aprovado como base | todas |
| Nova decisão: Fase 1 sem integrações | não depender de API/bloqueador | cadastro/importação manual e motor local | 1 |
| AHM.pdf: K1/K2/K3 | medir sucesso comercial | painel e baseline | 1, 5 |
| Última reunião: ICP, produto, perguntas e timing | estruturar base por produto | catálogo comercial | 1 |
| Última reunião: Envia/Pipedrive/RD têm API | integrar depois | conectores comerciais | 2 |
| Última reunião: transbordo técnico | evitar alucinação | roteamento especialista | 1, 3 |
| Última reunião: sugestão antes de contato | humano aprova oportunidade | fila de oportunidades | 1, 3, 5 |
| Demonstração Ethos/Bart | usar agente operador | loops e skills | 4, 5 |

## 11. Diretriz para SPECs e tasks

A Fase 1 deve ser detalhada em várias SPECs e tasks imediatamente executáveis. Nenhuma SPEC da Fase 1 pode conter dependência externa obrigatória. Quando um dado vier futuramente por API, a SPEC deve prever a fonte manual como caminho base e a integração como evolução da Fase 2.
