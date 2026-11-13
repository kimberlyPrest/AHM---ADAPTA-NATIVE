# Roteiro do teste do Bloco 1 da Fase 2 (owner — ~15 min)

**Versão**: 0.0.61 · **Data**: 13/11/2026 · **Preparado por**: Bart (insumos prontos na base)

**Insumos já criados pelo Bart** (não precisa criar nada):
- Lead **"PROVA Teste Bloco 1 Baixa"** (Shockwatch, área Cadeia do Frio) + oportunidade na trilha baixa **SEM fechamento**
- Lead **"PROVA Teste Bloco 1 Alta"** (produto de prova, complexidade Alta) + oportunidade na trilha alta **COM fechamento** (Exportação EUA, USD 50.000)

## Parte 1 — Campos de entrada no lead (F2-T01) — ~4 min

1. Abrir **Leads** e buscar o lead **"PROVA Teste Bloco 1 Baixa"**.
2. Conferir os campos novos gravados: **Área de interesse** = Cadeia do Frio, **País** = Brasil, **Idioma** = Português (pt-BR), **Modelo de compra** = Nacional.
3. Em **Editar**, alterar um deles (ex.: área → Segurança e Logística), salvar e conferir que gravou.
4. Abrir **Novo lead** e conferir os campos novos com os valores padrão (País "Brasil", Idioma "pt-BR").

## Parte 2 — Importação com as colunas novas (F2-T01) — ~3 min

1. Baixar o insumo: `06_notas/insumo-teste-f2-bloco1-importacao.csv` (2 linhas: 1 válida + 1 inválida).
2. Em **Leads → Importar CSV**, selecionar o arquivo.
3. **Esperado na prévia**: 2 linhas lidas, **1 inválida** com o motivo "Área de interesse inválida: Area Inventada (use Qualidade no Transporte, Cadeia do Frio ou Segurança e Logística)" e **1 novo**.
4. Importar. **Esperado**: toast "1 novo lead importado." (a linha inválida não grava).

## Parte 3 — Pipeline da oportunidade (F2-T02, T03, T04) — ~6 min

**3a. Trilha baixa SEM fechamento (bloqueio)** — abrir a oportunidade do lead **"PROVA Teste Bloco 1 Baixa"** (produto Shockwatch).
1. **Esperado**: nasceu na etapa **Problema Emergente** (1ª do AHM Cycle — 5 fases), passo a passo visível.
2. Tentar **Avançar para "Diagnóstico Técnico Compartilhado"** SEM preencher o Fechamento. **Esperado**: bloqueio com a mensagem "Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento."
3. Na seção **Fechamento**, escolher ambiente **Venda em reais** e valor 100.000. **Esperado**: "Moeda: BRL · Entidade vendedora: AHM Brasil (definidas automaticamente pelo ambiente de fechamento)". Salvar.
4. Avançar para a etapa 2. **Esperado**: avança; o passo a passo destaca a etapa atual com "Na etapa ... desde <data>".
5. **Voltar etapa** para a 1ª. **Esperado**: volta; abrir o **Histórico** e conferir os registros de passagem (1ª aberta na criação, 1ª fechada, 2ª fechada, 1ª reaberta) — nada é apagado.

**3b. Trilha alta COM fechamento (avançar e voltar)** — abrir a oportunidade do lead **"PROVA Teste Bloco 1 Alta"** (produto de prova, complexidade Alta).
1. **Esperado**: nasceu na etapa **Cenário Atual** (1ª das 8 etapas do Site Assessment), passo a passo com as 8 etapas visível.
2. **Esperado no Fechamento**: ambiente **Exportação (EUA)**, valor 50.000, "Moeda: USD · Entidade vendedora: AHM Solution Americas".
3. Avançar para **Diagnóstico** e depois **Voltar etapa**. **Esperado**: as duas trocas funcionam e o Histórico registra as passagens.

## Parte 4 — Métricas (F2-T09) — ~2 min

1. Abrir **Métricas** e conferir as fontes: **Landing Page** e **Outras origens** aparecem na lista.
2. Conferir que a soma das fontes fecha com o total (hoje: manual 1 + CSV 2.173 + Landing Page 1 + Outras 0 = 2.175).

## Limpeza (feita pelo Bart após o teste — não apague nada)

- O Bart apaga no fim: leads "PROVA Teste Bloco 1 Baixa/Alta", "Prova Roteiro Bloco 1" (da importação), o produto "PROVA Pipeline Alta" e as oportunidades de prova. Nada da base real é tocado.

## Resultado

- Tudo conforme → responder "bloco 1 ok" ao Bart.
- Qualquer desvio → descrever o passo e o comportamento observado.
