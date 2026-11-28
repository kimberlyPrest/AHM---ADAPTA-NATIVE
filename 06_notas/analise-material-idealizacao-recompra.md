# Material da idealização — rotina de recompra do cíclico · Bart · 28/11/2026 13:30

**Pedido do owner (28/11):** extrair o material da rotina de recompra da idealização do sistema, analisar e passar ao CC; o que faltar, grava com a Rose (reunião de segunda 30/11, com Cilene e Mario, gravada).
**Base usada:** emenda 1.1a (`04_fase-atual/specs/emenda-1.1a-modelos-diagnosticos-site-assessment.md`), mapa Mkt&Vendas consolidado (`artifacts/mapeamento-fluxo-mkt-vendas-ahm.md`), mapa Adm/Fin/ComEx/Log consolidado (`artifacts/mapeamento-fluxo-adm-fin-comex-log-ahm.md`), Bíblia de Vendas (KPIs). **O desenho v0 do CC (`06_notas/fluxo-recompra-ciclico.md`) está nos commits locais — não o li diretamente; a conferência abaixo usa o resumo do próprio CC e precisa ser reconciliada por ele.**

## 1. O que a idealização JÁ define (o CC pode usar direto)

| Elemento | Fonte | Conteúdo exato |
|---|---|---|
| Ciclo do cíclico | emenda 1.1a, tabela de modelos | **45–120 dias**; faturado 21 dd (negociável); fornecedor SpotSee; previsibilidade ALTA com **PUSH de recompra** |
| Campos de dados do ciclo | emenda 1.1a, "campos sugeridos" | `ciclo_dias_min`, `ciclo_dias_max`, `condicoes_pagamento`, `canal`, `fornecedores`, `previsibilidade` — **por MODELO de negócio, não por cliente** |
| Automação A5 | mapa Mkt&Vendas, seção 7 | "Alerta de recompra PUSH — gatilho: ciclo do produto (45–120d) expirando — destino: tarefa/notificação para o responsável" |
| Fase de pós-venda que planeja a recompra | mapa Mkt&Vendas, trilha AHM Cycle | "Acompanhamento e Expansão → **planejamento do próximo ciclo de compra**"; Gestão de Sucesso → "renovação automática com benefícios" |
| Aprimoramentos já sugeridos | emenda 1.1a + consolidação | gatilho automático de recompra por ciclo (45–120d) + **alerta de expiração de indicador** (o produto monitorado "vence" — liga com WarmMark/FreezeSafe QR etc.); expansão da fase 5 alimentando a carteira PUSH |
| Meta de produtividade | Bíblia de Vendas (KPIs) | **9 vendas cíclicas / 1 pontual por consultor** — a recompra é o motor da carteira |
| Regra transversal | emenda 1.1a | "documentar tudo no CRM" — o registro de ciclo mora no CRM |

**Leitura:** a idealização define o QUANDO (ciclo 45–120d por modelo) e o QUE (alerta PUSH + planejamento do próximo ciclo na fase de acompanhamento). Não define o COMO operacional por cliente — é exatamente o que o desenho v0 do CC está fazendo.

## 2. O que a idealização NÃO define (gaps — grava com a Rose na segunda)

1. **Início do ciclo por cliente.** A emenda dá a faixa por modelo; a rotina precisa da data de partida de cada cliente (última compra? entrega? ativação?). Perguntar: *"Como você sabe hoje que o cliente X está perto de recomprar — de que data você parte?"*
2. **Ciclo real por cliente/produto.** 45–120d é a faixa do modelo; clientes reais recompram em ritmos próprios. Perguntar: *"Tem cliente que recompra todo mês? Outro que demora 6 meses? O que explica a diferença (consumo, sazonalidade)?"*
3. **Prazo de reposição por produto.** O v0 usa "importação 4–6 semanas" (estimativa do CC). Validar: *"Para os produtos cíclicos (SpotSee etc.), quanto tempo da ordem à entrega? E tem produto nacional com reposição mais rápida?"* (Mario/Cilene ajudam no prazo de importação.)
4. **Preço na recompra.** O owner decidiu "preço travado por 1 ano = candidato a automático (conferente avisa margem baixa por câmbio)" — regra NOVA, sem base na idealização. Perguntar: *"Como você precifica a recompra hoje — tabela, última venda, câmbio do dia? Já travou preço por 1 ano com algum cliente?"*
5. **Consumo mensal do cliente.** Owner: vira pergunta obrigatória do diagnóstico. Validar: *"Você sabe hoje quanto cada cliente consome por mês? Onde isso está registrado?"*
6. **O que acontece quando o cliente NÃO recompra.** O v0 marca "em risco" a 1,5× ciclo — regra nova. Perguntar: *"Quando o cliente passa do ciclo e não compra, o que aconteceu com esses clientes? Perderam para concorrente? O produto ainda está em uso?"*
7. **Programa anual.** Ideia do owner (contrato anual de recompra). Perguntar: *"Já existe algo parecido hoje — compromisso de compra anual, desconto por volume comprometido?"*
8. **Renovação "com benefícios".** O mapa fala em "renovação automática com benefícios" sem definir quais. Perguntar: *"Que benefício faz sentido dar para quem recompra no prazo (frete, preço, prioridade)?"*

## 3. Conferência do desenho v0 do CC contra o material (reconciliar)

| Elemento do v0 (resumo do CC) | Contra a idealização |
|---|---|
| Trilha curta de 3 etapas | **Nova** — a idealização tem o AHM Cycle de 5 fases para a VENDA, mas não desenha trilha de recompra; a trilha curta é o acerto do v0 (não conflita: a fase "planejamento do próximo ciclo" do mapa vira a porta de entrada dela) |
| Registro de ciclo | **Estende** os campos da emenda (ciclo_dias_min/max por modelo) para registro POR cliente — extensão correta, documentar a diferença |
| Aviso considera prazo de reposição (4–6 semanas) | **Novo e necessário** — sem isso o alerta chega tarde; prazo é estimativa, validar na reunião (gap 3) |
| 3 portas de entrada | Não li o arquivo; o CC reconcilia com o item "planejamento do próximo ciclo" + A5 + gatilho de expiração de indicador (os 3 gatilhos que a idealização já tem) |
| "Em risco" a 1,5× ciclo | **Nova** — sem base na idealização; validar com a Rose (gap 6) antes de virar regra |
| Preço 1 ano / consumo mensal / programa anual | Decisões do owner de 28/11 — regras novas; consumir os gaps 4, 5 e 7 |

## 4. Riscos que encontrei além do pedido

1. **(MÉDIA) O aviso de recompra sem o registro de ciclo por cliente vira lembrete genérico.** A automação A5 só funciona depois do registro por cliente existir e estar alimentado — a rotina de segunda precisa terminar com as contas ativas mapeadas (quem são os clientes cíclicos hoje e de que data parte cada um). Sem isso, o primeiro agente de recompra nasce cego.
2. **(BAIXA) Cilene sai na segunda e o faturamento vira agente** — o mesmo registro de ciclo alimenta o faturamento recorrente (NF de serviço); garantir que o desenho da recompra e o desenho do agente de faturamento leiam a MESMA fonte (contratos/locações), não duas.
3. **(BAIXA) "Renovação automática com benefícios" não pode ser automática antes de existir a regra de benefício** — até lá, o que é automático é o AVISO, não a renovação (coerente com a matriz de autonomia: rascunho primeiro).
4. **(INFORMAÇÃO) Julio fica só venda; Rose assume supervisão dos agentes de Adm de Vendas** — o par executor×conferente da recompra reporta a ela; registrar no padrão R1 (humano_responsavel).

## 5. Passagem ao CC

O que o CC já pode usar: seção 1 (toda). O que fica aguardando a reunião de segunda: os 8 gaps da seção 2 — a gravação com a Rose/Mario/Cilene fecha o material e o desenho v0 sobe para a versão 1 com as respostas. O roteiro da reunião (passagem de bastão da Cilene) é do CC; os gaps acima são as perguntas de recompra a embutir nele.
