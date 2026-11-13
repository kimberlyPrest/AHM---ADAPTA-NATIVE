# Roteiro do teste do Bloco 1 da Fase 2 (owner — ~15 min)

**Versão**: 0.0.61 · **Data**: 13/11/2026 · **Preparado por**: Bart (insumos prontos)

## Parte 1 — Campos de entrada no lead (F2-T01) — ~4 min

1. Abrir **Leads → Novo lead**.
2. Conferir os campos novos: **Área de interesse** (3 opções: Qualidade no Transporte, Cadeia do Frio, Segurança e Logística), **Modelo de compra** (Nacional / Importação), **País** (preenchido com "Brasil"), **Idioma** (preenchido com "Português (pt-BR)").
3. Criar um lead de teste: nome "Teste Bloco 1", empresa qualquer, e-mail seu, área **Cadeia do Frio**, modelo **Nacional**. Salvar.
4. Abrir o lead criado em **Editar** e conferir que os 4 campos gravaram.

## Parte 2 — Importação com as colunas novas (F2-T01) — ~3 min

1. Baixar o insumo: `06_notas/insumo-teste-f2-bloco1-importacao.csv` (2 linhas: 1 válida + 1 inválida).
2. Em **Leads → Importar CSV**, selecionar o arquivo.
3. **Esperado na prévia**: 2 linhas lidas, **1 inválida** com o motivo "Área de interesse inválida: Area Inventada (use Qualidade no Transporte, Cadeia do Frio ou Segurança e Logística)" e **1 novo**.
4. Importar. **Esperado**: toast "1 novo lead importado." (a linha inválida não grava).

## Parte 3 — Pipeline da oportunidade (F2-T02, T03, T04) — ~6 min

1. Abrir **Oportunidades** e criar uma oportunidade para o lead "Teste Bloco 1" (produto **Shockwatch** — baixa complexidade).
2. **Esperado**: a oportunidade nasce na etapa **Problema Emergente** (1ª do AHM Cycle — 5 fases), com o passo a passo visível.
3. Tentar **Avançar para "Diagnóstico Técnico Compartilhado"** SEM preencher o Fechamento. **Esperado**: bloqueio com a mensagem "Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento."
4. Na seção **Fechamento**, escolher ambiente **Venda em reais** e valor 100.000. **Esperado**: "Moeda: BRL · Entidade vendedora: AHM Brasil (definidas automaticamente pelo ambiente de fechamento)". Salvar.
5. Avançar para a etapa 2. **Esperado**: avança; o passo a passo destaca a etapa atual com "Na etapa ... desde <data>".
6. **Voltar etapa** para a 1ª. **Esperado**: volta; abrir o **Histórico** e conferir 3 registros (1ª etapa aberta, 1ª fechada, 2ª fechada, 1ª reaberta) — nada é apagado.

## Parte 4 — Métricas (F2-T09) — ~2 min

1. Abrir **Métricas** e conferir as fontes: **Landing Page** e **Outras origens** aparecem na lista.
2. Conferir que a soma das fontes fecha com o total (hoje: manual 1 + CSV 2.173 + Landing Page 1 + Outras 0 = 2.175).

## Limpeza

- Apagar os leads "Teste Bloco 1" e "Prova Roteiro Bloco 1" criados no teste (a oportunidade vai junto em cascata). A linha inválida não gravou nada.

## Resultado

- Tudo conforme → responder "bloco 1 ok" ao Bart.
- Qualquer desvio → descrever o passo e o comportamento observado.
