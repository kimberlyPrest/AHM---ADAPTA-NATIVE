# Roteiro do teste do Bloco 1 da Fase 2 (owner — ~15 min)

**Versão**: 0.0.61 · **Data**: 13/11/2026 · **Preparado por**: Bart (insumos prontos)

## Preparado pelo Bart (não precisa criar nada)

- Lead **"PROVA Teste Bloco 1 Baixa"** (Empresa Prova Roteiro) com oportunidade **na trilha baixa (AHM Cycle, 5 fases)**, na etapa 1 "Problema Emergente", **sem Fechamento preenchido** — serve para o bloqueio e para avançar/voltar.
- Lead **"PROVA Teste Bloco 1 Alta"** com oportunidade **na trilha alta (Site Assessment, 8 etapas)**, na etapa 1 "Cenário Atual", **com Fechamento já salvo** (Exportação EUA, USD, AHM Solution Americas, valor 50.000) — serve para conferir a trilha alta e o derivado.
- Insumo da importação: `06_notas/insumo-teste-f2-bloco1-importacao.csv` (1 linha válida + 1 inválida).

## Parte 1 — Campos de entrada no lead (F2-T01) — ~3 min

1. Abrir **Leads → Novo lead**.
2. Conferir os campos novos: **Área de interesse** (Qualidade no Transporte, Cadeia do Frio, Segurança e Logística), **Modelo de compra** (Nacional / Importação), **País** (preenchido com "Brasil"), **Idioma** (preenchido com "Português (pt-BR)").
3. Ao escolher a área, conferir a **sugestão de famílias** (ex.: Cadeia do Frio sugere a família de indicadores de cadeia do frio).
4. Salvar um lead novo com área **Cadeia do Frio** e modelo **Nacional**; abrir em **Editar** e conferir que os 4 campos gravaram.

## Parte 2 — Importação com as colunas novas (F2-T01) — ~3 min

1. Baixar o insumo `06_notas/insumo-teste-f2-bloco1-importacao.csv` (2 linhas: 1 válida + 1 inválida).
2. Em **Leads → Importar CSV**, selecionar o arquivo.
3. **Esperado na prévia**: 2 linhas lidas, **1 inválida** com o motivo "Área de interesse inválida: Area Inventada (use Qualidade no Transporte, Cadeia do Frio ou Segurança e Logística)" e **1 novo**.
4. Importar. **Esperado**: toast "1 novo lead importado." (a linha inválida não grava).

## Parte 3 — Pipeline na trilha baixa (F2-T02, T03, T04) — ~5 min

1. Abrir **Oportunidades** → abrir a oportunidade do lead **"PROVA Teste Bloco 1 Baixa"** (produto Shockwatch).
2. **Esperado**: passo a passo do **AHM Cycle com 5 fases**, na etapa 1 **"Problema Emergente"**.
3. Tentar **Avançar** SEM preencher o Fechamento. **Esperado**: bloqueio — "Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento."
4. Na seção **Fechamento**: ambiente **Venda em reais**, valor **100000**. **Esperado**: "Moeda: BRL · Entidade vendedora: AHM Brasil (definidas automaticamente pelo ambiente de fechamento)". Salvar.
5. **Avançar** para a etapa 2. **Esperado**: avança; destaque "Na etapa ... desde <data de hoje>".
6. **Voltar etapa** para a 1ª. **Esperado**: volta; no **Histórico**, conferir os registros com datas e responsável — nada é apagado (1ª etapa aberta, 1ª fechada, 2ª aberta e fechada, 1ª reaberta).

## Parte 4 — Trilha alta (F2-T03) — ~2 min

1. Abrir a oportunidade do lead **"PROVA Teste Bloco 1 Alta"** (produto PROVA Pipeline Alta).
2. **Esperado**: passo a passo do **Site Assessment com 8 etapas**, na etapa 1 "Cenário Atual".
3. Conferir o Fechamento já preenchido: **Exportação (EUA) · Moeda: USD · Entidade: AHM Solution Americas · Valor: 50.000**.
4. **Avançar** para "Diagnóstico" e **Voltar** — deve funcionar (fechamento já está completo); conferir o Histórico.

## Parte 5 — Métricas (F2-T09) — ~2 min

1. Abrir **Métricas** e conferir as fontes: **Landing Page** e **Outras origens** aparecem na lista.
2. Conferir que a soma das fontes fecha com o total (hoje: manual 1 + CSV 2.173 + Landing Page 1 + Outras 0 = 2.175).

## Limpeza (o Bart faz no fim — você não precisa apagar nada)

- O Bart apaga só os registros **PROVA** criados no teste (2 leads + 2 oportunidades + lead da importação + produto de prova) e confirma a base real intacta.

## Resultado

- Tudo conforme → responder "bloco 1 ok" ao Bart.
- Qualquer desvio → descrever o passo e o comportamento observado.
