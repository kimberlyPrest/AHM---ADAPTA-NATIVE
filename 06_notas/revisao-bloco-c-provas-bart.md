# Revisão do Bloco C (0.0.72) — provas do Bart · 13/11/2026 (noite)

**Protocolo aplicado**: pré-mortem com 3 riscos provados · regressão por papel · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

## Resultado: APROVADO — provas de API e tela verdes; 1 ressalva de processo (divergência 25×31 motivos) e 2 pendências menores

## Prioridade 1 — rota /backend/v1/desempenho com login (o CC não pôde rodar)

- **Admin, sem params**: 200 em 0,12s — payload completo (taxaVitoria, funil, abertas, porMes, perdasPorEtapa, abertasPorEtapa, ticketMedio, duracao, emNegociacao, tempoPorEtapa, porResponsavel, meta, naoIniciadas, perdasPorMotivo)
- **Admin, 5 períodos × 3 trilhas** (7/30/90/180/365 × alta/baixa/todas): **15/15 → 200**, sem nenhum 400
- **Vendedor (Julio), 6 combinações**: todas 200
- **Sem login**: 401 (o esperado, como o CC disse)

## Provas do pedido

| Prova | Resultado |
|---|---|
| (a) Vendedor pede `responsavel=<id de outro>` | ✅ O servidor **força o próprio id** do vendedor no filtro (pediu Rose `na9v1yo7bre4dhi`, respondeu com `responsavel: voervp32o7igae0` = Julio). Admin pede responsável de outro → respeitado (`admin: true`) |
| (b) Perda sem motivo → 400 | ✅ "Escolha o motivo da perda na lista." · Motivo inativo ("Outro") → 400 "O motivo de perda escolhido não está disponível." · Motivo ativo → 200 + histórico gravado (fechamento c/ data e motivo) |
| (b-extra) "Outro" ativado + perda SEM observação | ⚠️ **PASSOU quando devia bloquear** — ver D1 abaixo (baixa) |
| (c) Vendedor troca responsável → 403 | ✅ "Somente um administrador pode alterar o responsável da oportunidade." · Vendedor edita motivos_perda → 403 · Vendedor edita configurações (meta) → 403 — mensagens corporativas nos 3 |
| (d) Moedas separadas | ✅ Conferido contra a base: abertas BRL 250.000 (2 ops) / USD 130.000 (2 ops) / sem valor 4 — a rota devolve exatamente isso, nunca somadas; tabela por responsável tem colunas Ganho (R$) e Ganho (US$) separadas |
| (e) Lead sem responsável → "Sem responsável" | ✅ Aparece na tabela por responsável (linha "Sem responsável": 1 criada, 1 aberta) e no filtro `responsavel=sem` (1 op sem valor) |
| (f) /tickets e /atendimentos continuam abrindo | ✅ Ambas 200 no preview (fora do menu, mas vivas) |
| (g) Minha mesa e Desempenho na tela (admin) | ✅ Ver seção "Tela" abaixo |

## Tela (como admin, browser)

- **Minha mesa** (`/`): saudação "Olá, Afonso!", botão "Novo lead" (2x), resumo do mês, oportunidades abertas cada uma com o próximo passo (incl. "PROVA AB B2 … Sem responsável"), "Ir para Leads"/"Ir para Transbordos", "Ver todas"
- **Menu**: Minha mesa · Leads · Oportunidades · Transbordos · Métricas · Desempenho · Produtos · Equipe — **Tickets e Atendimentos fora** ✓
- **Desempenho** (`/desempenho`): 5 botões de período, combos trilha (3 opções) e responsável (Time inteiro + Sem responsável + 8 pessoas), 4 gráficos renderizados (criados×meta, abertas por etapa, perdas por etapa, tempo por etapa), tabela por responsável (Afonso Moreira 7 criadas/6 abertas; Sem responsável 1/1), painéis de taxas de passagem do Bloco 2 (baixa e alta, real×alvo)
- **Configurações**: meta 20 no campo + "Salvar meta", 26 checkboxes de motivos (25 ativos + "Outro (especificar) — exige observação" desmarcado), toggle do "Outro" funcionando com toasts "Motivo ativado"/"Motivo desativado"

## Pré-mortem — 3 riscos

1. **Rota de desempenho quebra com filtros combinados** → PROVADO: 15/15 combinações admin + 6 vendedor, todas 200. ✓
2. **Lista de motivos com furo de validação** → PROVADO PARCIALMENTE: sem motivo e motivo inativo bloqueiam; **"Outro" ativo sem observação passou** (D1 abaixo). ⚠️
3. **Vendedor ver dados do time via API** → PROVADO: servidor força o próprio id; porResponsavel do vendedor vem vazio (só admin vê a tabela). ✓

## Dados inventados = defeito (varredura)

- Desempenho sem dados: "—" nas células, taxaVitoria null, ticketMedio null — **nenhum número inventado** ✓
- Funil mostra números reais da base (2.193 leads recebidos → 9 qualificados → 8 oportunidades → 4 aprovadas → 2 em venda → 0 ganhas) ✓
- Meta 20 vem da coleção `configuracoes` (não hardcoded) ✓

## Logs

- Nenhum warning `e.next()` novo (último: 12/11 12:53 — antes da correção) ✓
- Erros de hoje = recusas esperadas das próprias provas (403 de permissão, 400 de validação) + o 401 que o CC citou ✓
- O erro antigo (vendedor pedindo histórico de fechamentos) não reapareceu durante as provas ✓

## Riscos que encontrei além do pedido

1. **D1 (baixa)**: com "Outro (especificar)" ATIVO, registrar perda com ele e observação vazia **passou (200)** — o `exige_detalhe` não está sendo cobrado no hook de oportunidade. O histórico gravou "fechamento" sem observação. Reproduzível: ativar Outro → PATCH resultado=perdida + motivo_perda=Outro + resultado_motivo="" → 200. **Correção pequena** no hook de fechamento (cobrar `exige_detalhe` quando o motivo está ativo). Não bloqueia o bloco (o motivo inativo bloqueia; o fluxo da tela pede a observação), mas é a regra prometida.
2. **Histórico de prova na op PROVA AB B2 (baixa)**: minhas provas gravaram 1 fechamento + 1 reabertura no histórico dela (imutável por desenho). Estado da op restaurado (sem fechamento, como estava). Sem impacto em dados reais.
3. **Painel de taxas de passagem sem filtro de responsável** (o CC já avisou; confirmado na tela) — comportamento documentado, OK por enquanto.
4. **Divergência 25×31 (processo, meu)**: o título do meu briefing dizia "31 motivos" mas a lista tinha 25 — erro de contagem meu. Os 25 corretos entraram + "Outro". Se faltar motivo na visão do owner, ativa pela tela de Configurações ou me passa.

## Limpeza / estado final

- Op PROVA AB B2 restaurada (sem fechamento, sem resultado) — é insumo do teste do owner
- "Outro (especificar)" restaurado para inativo
- Meta permanece 20 (não alterada)
- Nenhum lead/oportunidade de prova novo criado (provei tudo sobre a op PROVA existente)

## Conclusão

**Bloco C APROVADO** (0.0.72): rota de desempenho sólida (21 combinações verdes), isolamento por papel server-side provado, validações de perda corretas (exceto D1 baixa), moedas separadas conferidas contra a base, telas novas funcionando sem dado inventado, logs limpos. Pendências: correção do D1 (pequena), feedback da Julio/Rose, senha do owner.
