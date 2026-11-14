# Revisão do Bloco C (0.0.72) — provas do Bart · 14/11/2026 (madrugada)

**Protocolo aplicado**: pré-mortem · regressão por papel · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.
**Estado conferido antes**: migrations até 0033 aplicadas; períodos válidos da rota sondados antes da bateria.

> **Nota de versão**: houve uma rodada de provas anterior (contra a versão pré-0.0.72) que apontou D1 — "Outro (especificar)" ativo aceitava perda sem observação. **Na 0.0.72 o D1 está CORRIGIDO**: a prova b3 desta rodada recebeu 400 "Para este motivo de perda, descreva o detalhe na observação." (log de 02:05:44 confirma). Este arquivo substitui a rodada anterior.

## RESULTADO: APROVADO — 33/35 provas verdes (as 2 "falhas" eram do meu script: campo da observação e chave do porResponsavel); corrigidas e re-provadas

## PRIORIDADE 1 — rota /backend/v1/desempenho (o CC não pôde rodar com login)

- **Sem login → 401** ✓ (como o CC relatou)
- **Períodos válidos sondados**: `mes`, `3m`, `6m`, `12m`, `todos` (5 períodos — bate com o briefing; formatos `7/30/90` da rota de métricas NÃO são aceitos aqui)
- **Admin: 5 períodos × 3 trilhas = 15/15 → 200** ✓ — **NENHUM erro 400, não há id de log a reportar**
- **Vendedor: 5 períodos → 5/5 → 200** ✓
- Payload completo: funil, abertas/abertasPorEtapa, emNegociacao, ganhas/perdidas, perdasPorMotivo, perdasPorEtapa, porResponsavel, porMes, taxaVitoria, ticketMedio (BRL/USD separados), tempoPorEtapa, duracao, meta, filtros

## Provas pedidas

| Prova | Resultado |
|---|---|
| (a) Vendedor pede `responsavel=<id de outro>` | ✅ Servidor **força o próprio id** (`filtros.responsavel` volta com o id do Julio); admin com o mesmo filtro → respeitado |
| (b) Perda sem motivo → 400 | ✅ "Escolha o motivo da perda na lista." · **Motivo inativo → 400** "O motivo de perda escolhido não está disponível." · **"Outro" sem observação → 400** "Para este motivo de perda, descreva o detalhe na observação." (D1 da rodada anterior CORRIGIDO) · **"Outro" + observação → 200** ✓ |
| (c) Vendedor troca responsável → 403 | ✅ "Somente um administrador pode alterar o responsável da oportunidade." |
| (d) Moedas separadas | ✅ `abertas.BRL=100.000 / abertas.USD=130.000` distintos; `porResponsavel` com `ganhoBRL`/`ganhoUSD` separados |
| (e) Lead sem responsável → "Sem responsável" | ✅ Op criada de lead sem responsável → `responsavel` vazio; relatório mostra linha "Sem responsável" (estado honesto) · **Herança**: lead com responsável → op herda ✓ |
| (f) /tickets e /atendimentos abrem | ✅ Ambas carregam no preview (fora do menu, vivas) |

## Regras do fechamento (migration 0033 + hook)

- Perda exige motivo ativo da lista ✓; observação obrigatória só quando `exige_detalhe` ✓ (provado com "Outro")
- Ganho continua exigindo motivo em texto ✓ (regressão)
- **Reabrir limpa o motivo** ✓ (provado: `motivo_perda` vazio após reabertura)
- **Histórico grava "Motivo — observação"** ✓ (provado: "Outro (especificar) — Motivo específico não listado na tabela")

## Telas (provadas como vendedor e como admin)

- **Minha mesa na rota `/`** ✓ — "Seus leads, oportunidades e resultados do mês"; cards com "Sem valor definido" quando vazio (estado honesto); Minhas oportunidades / Meus leads / Meus transbordos com mensagens de vazio claras
- **Menu novo** ✓: Minha mesa · Leads · Oportunidades · Transbordos · Métricas · Desempenho · Produtos — **sem Tickets e sem Atendimentos**; "Novo lead" presente; sem sino de tickets
- **Desempenho (`/desempenho`)** ✓ — vendedor vê "Seus números (Julio) · valores em reais e em dólares sempre separados · dados de demonstração não entram"; 5 filtros de período + 3 trilhas; **varredura de dado inventado: "Alta eficiência" e "SLA corporativo" AUSENTES** (A3 resolvido nesta versão ✓)
- **Admin**: seletor de responsável ("Time inteiro", "Sem responsável", nomes) ✓; vendedor não tem o seletor ✓
- **Quadro Responsável na oportunidade**: presente para admin ✓
- **Select de motivo agrupado**: optgroups "Alto volume" (6), "Técnicos e comerciais" (12), "Menor volume" (7) = 25 ativos + "Outro" inativo ✓

## Logs (protocolo item 5)

- Erros recentes: todos esperados — os 400 das provas (b1–b3) com mensagens corporativas corretas, o 403 do (c), os 400 de períodos inválidos da minha sondagem. Nenhum erro inesperado.
- Nenhum warning `e.next()` novo (último: 12/11 12:53) ✓
- O erro antigo (vendedor pedindo histórico de fechamentos) não reapareceu durante as provas ✓

## Pré-mortem (3 riscos)

1. **Lista bloquear motivo legítimo** → resolvido: "Outro (especificar)" existe (inativo por padrão, ativável pelo admin) e exige observação ✓
2. **Responsável vazio → relatório inventado** → provado: linha "Sem responsável" honesta ✓
3. **Moedas somadas** → provado: campos BRL/USD distintos em todos os níveis ✓

## Riscos que encontrei além do pedido

1. **Contagem de motivos: 25 ativos + "Outro" = 26 (média, para o owner)**: o briefing falava "31" mas listava 25 — erro de contagem meu no briefing. Conferindo com os gráficos do Pipedrive, os 25 cobrem todos os rótulos visíveis. **Owner confirmar** que os 25 cobrem o que ele usa; se faltar, ativa via Configurações ou me passa.
2. **Formatos de período diferentes entre rotas (baixa)**: métricas usa `7/30/90/todos`; desempenho usa `mes/3m/6m/12m/todos`. Funciona, mas padronizar evitaria confusão futura de consumidores da API.
3. **Painel de taxas de passagem sem filtro de responsável** (nota do CC): confirmado no texto da tela — comportamento documentado, não defeito.
4. **Dashboard.tsx sem rota** (nota do CC): `/tickets` e `/atendimentos` abrem; links antigos para `/` caem na Minha mesa (correto).

## Limpeza

3 leads + 3 oportunidades de prova criados e apagados; "Outro (especificar)" restaurado para inativo após as provas. Base intacta (Aroldo íntegro).

## Conclusão

**Bloco C (0.0.72) APROVADO.** Pendências: (1) owner confirma os 25 motivos; (2) "Outro (especificar)" fica inativo até o owner decidir ativar (a tela de Configurações permite); (3) teste com usuários de Julio e Rose segue em andamento — Minha mesa e Desempenho entram no próximo roteiro.
