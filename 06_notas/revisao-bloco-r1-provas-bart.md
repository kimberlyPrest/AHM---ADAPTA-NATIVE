# Revisão do Bloco R1 (0.0.76) — provas do Bart · 24/11/2026

**Estado conferido em 24/11 12:50: versão 0.0.76, última migration 0037, working tree limpo, último commit 086e7dc (repo).** Protocolo aplicado: pré-mortem, regressão por papel, varredura, dado inventado = defeito, logs, riscos além do pedido.

## Resultado: APROVADO COM 1 DEFEITO A CORRIGIR (A1) + 2 observações — o resto, 100% verde

## Provas do pedido (API + tela)

| Prova | Resultado |
|---|---|
| P1: lead PROVA qualificado → op criada → `situacao_comercial` = "em negociação"; sai de "para trabalhar" | ✅ (Minha mesa sem o lead; fila com 30 leads sem o PROVA) |
| P2: ganho → "ganho"; reabrir → "em negociação"; perda → "perdido" | ✅ (ganho exigiu `resultado_motivo`; perda exigiu `motivo_perda` da lista + texto; reabertura exigiu `reabertura_motivo`) |
| P3: op ganha + op aberta → "em negociação"; rejeitar a aberta (com comentário novo) → "ganho" | ✅ (rejeitada não conta, como desenhado) |
| P4: excluir as ops → lead volta a "sem oportunidade" | ✅ |
| Lead novo (API) nasce "sem oportunidade" | ✅ |
| Backfill: Eduardo Zuin = "em negociação" (1 op aberta) ✅ · Veronica Ruiz = "ganho" ✅ · Aroldo (5 registros) = "sem oportunidade" ✅ | ✅ |
| R1.3: troca de produto do lead (status igual) → op aberta muda de produto + nota no comentário + trilha recalculada (baixa→alta) | ✅ — nota: "Produto alterado de Shockwatch para PROVA R1 Alta… a partir do cadastro do lead em 24/11/2026." |
| R1.3: op FECHADA (ganha) não muda | ✅ (manteve produto/trilha alta) |
| R1.4: avanço com data de hoje → 200; `etapa_ocorrido_em` não persiste; `data_informada=false` | ✅ |
| R1.4: data futura → 400 "não pode ser futura" | ✅ |
| R1.4: data anterior à entrada na etapa anterior → 400 com a data na mensagem | ✅ |
| R1.4: sem data → agora, `data_informada=false` | ✅ |
| Regressão: perda sem motivo → 400; avançar sem fechamento → 400; vendedor fora de config → 403/404; excluir lead real → 403; excluir lead PROVA → permitido | ✅ |
| Tela: selos na lista de leads ("Qualificado · Em negociação"; Veronica Ruiz "Qualificado · Venda ganha") | ✅ |
| Tela: Minha mesa sem leads ganhos/perdidos (Veronica não aparece) | ✅ |
| Linguagem das mensagens novas: pt-BR corporativo, sem códigos internos | ✅ |

## DEFEITO A1 (médio — corrigir antes de concluir): `etapa_ocorrido_em` persiste quando enviado na CRIAÇÃO

- **Reprodução**: POST de oportunidade com `etapa_ocorrido_em = 3 dias atrás` → 200; o hook de CRIAÇÃO não lê nem limpa o campo → ele **persiste** no registro (o hook de UPDATE limpa, o de criação não).
- **Efeito em cadeia**: o hook de UPDATE lê o valor "atual" do record (não o payload) — com o campo sujo, **todo avanço de etapa futuro falha** com "A data em que a etapa aconteceu não pode ser anterior à entrada na etapa anterior", até alguém limpar o campo manualmente. Provas: avanço sem data → 400; após limpar o campo explicitamente → 200.
- **Correção sugerida ao CC**: no `onRecordCreate` do pipeline, limpar `etapa_ocorrido_em` (como o update faz) — 1 linha.
- **Nota de desenho (não bloqueia)**: com a regra atual, o caso retroativo da Rose (lançar hoje um ciclo completo da semana passada) continua parcial: a 1ª etapa nasce sempre com data de hoje, então qualquer data passada informada depois é recusada. Se o owner quiser o caso completo, é uma decisão de desenho (aceitar `etapa_ocorrido_em` na criação com validação própria), não um bug.

## Observações (baixas)

1. **Realtime da situação comercial** (limitação declarada pelo CC, confirmada): a gravação por SQL não empurra atualização às telas abertas — recarregar atualiza. Aceitável; se incomodar, o CC já propôs a troca por gravação com evento.
2. **Troca de produto em lead qualificado exige `qualificacao_resultado`** (hook de qualificação) — comportamento correto e auditável; a tela já envia. Minha 1ª tentativa sem o campo falhou com mensagem clara.

## Riscos que encontrei além do pedido

- **(baixa) A1 acima** — o único achado novo.
- **(nota) `PROVA Pipeline Alta` está Inativo no catálogo** (residual de prova antiga) — sugiro limpeza pelo CC na próxima rodada.
- **(nota) Base total agora 15.406 leads** (15.370 reais + casos reais da Rose + ajustes) — os 2 casos da Rose ficaram como registros reais (decisão do owner pendente no STATUS).

## Logs e limpeza

- Logs: nenhum warning/erro novo após o deploy (conforme o CC declarou; não vi erro nos meus testes além dos 400 esperados).
- **Limpeza feita**: 4 oportunidades + 10 leads PROVA R1 + 1 produto PROVA R1 Alta apagados; leads reais intactos (Eduardo Zuin "em negociação", Veronica "ganho", Aroldo "sem oportunidade"); base 15.406.

## R1–R10

| Req | Situação nesta entrega |
|---|---|
| R2 Regra no servidor | ✅ situação comercial e troca de produto valem pela API (provas feitas só via API) |
| R5 Auditoria | ✅ nota da troca de produto no comentário; `data_informada` separa data informada × data do registro |
| R6 Data do fato | 🟡 parcial — A1 (campo persiste na criação); o resto verde |
| Demais | Não tocados nesta entrega |

## Conclusão

**BLOCO R1: APROVADO COM A CORREÇÃO A1 PENDENTE** (1 linha no hook de criação). B1, B2, B3 e B4 da Rose estão resolvidos e provados. Sugiro: CC corrige A1 → eu re-provo só o cenário A1 (2 provas) → "pode concluir".

**Próximo passo**: CC corrige A1 e me avisa.
