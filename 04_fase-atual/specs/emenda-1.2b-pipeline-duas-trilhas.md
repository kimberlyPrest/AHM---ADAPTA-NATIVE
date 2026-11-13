# EMENDA 1.2b — Pipeline de duas trilhas por complexidade do produto (Fase 2)

**Data:** 2026-11-12
**Origem do sinal:** Afonso Moreira (owner AHM Solution) — correção ao briefing da Fase 2 (12/11 21:40): "as etapas do pipeline precisam respeitar o modelo de negócio de baixa e alta complexidade — são etapas do Site Assessment diferentes, as quais já informei previamente".
**Status:** RATIFICADA pelo owner em 12/11/2026 (21:48, formulário de decisões da Fase 2 — opção "Duas trilhas pela complexidade")
**SPECs afetadas:** SPEC 1.5 (fila de oportunidades), SPEC 1.6 (KPIs — taxas de passagem), nova SPEC 1.8 (pipeline de duas trilhas)
**Fonte canônica:** emenda 1.1a §4 (Site Assessment 8 etapas × 3 fases; AHM Cycle 5 fases com mapeamento sobre as 8 etapas)

---

## 1. Princípio

O pipeline da Fase 2 NÃO é uma lista única de etapas: a oportunidade segue a **trilha de etapas definida pela complexidade do produto de interesse** (campo `complexidade` do produto — "Baixa" ou "Alta", já gravado no catálogo):

| Complexidade do produto | Trilha de etapas | Fases do fluxo |
|---|---|---|
| **Baixa** (8 produtos: ShockWatch, Thermax, WarmMark, FreezeSafe, HI Cards, Farol Blue, Red Light, TiltWatch) | **AHM Cycle — 5 fases**: Problema Emergente → Diagnóstico Técnico Compartilhado → Proposta Customizada → Implementação Piloto → Acompanhamento/Expansão | Pré-venda (1) · Venda (2-3) · Pós-venda (4-5) |
| **Alta** (12 produtos: SpotBot, ShockLog, Push-Pull, Data Loggers, Hit-Not, Iris860, Iris-i, Proxicam, Câmera Empilhadeira, Orlaco/Motec, Hit Not Report, Trocador de Paletes) | **Site Assessment — 8 etapas**: Cenário Atual → Diagnóstico → Refinamento → Apresentação de Projeto → Negociação → Implementação → Realidade → Adoção Corporativa | Pré-venda (1-3) · Venda (4-5) · Pós-venda (6-8) |

**Mapeamento AHM Cycle sobre o Site Assessment (emenda 1.1a §4):** fases 1-2 ≈ etapas 1-2 · fase 3 ≈ etapas 3-5 · fase 4 ≈ etapa 6 · fase 5 ≈ etapas 7-8 — permite comparar taxas de passagem entre as trilhas e reusar os critérios de conclusão por etapa (perguntas de reflexão como campos do registro, fatores de sucesso como critérios, lista de insucessos como alerta).

## 2. Regras de negócio

- **RN-2.1:** a trilha é definida pela `complexidade` do produto da oportunidade e não muda manualmente.
- **RN-2.2:** a etapa atual fica gravada na oportunidade com **data de entrada na etapa** (histórico completo de passagem por etapa — decisão do owner no formulário).
- **RN-2.3:** as taxas de passagem alvo por etapa são as da emenda 1.1a (60%–100%, tabela §4); o painel da Fase 2 compara real × alvo.
- **RN-2.4:** critérios de conclusão da etapa (emenda 1.1a + Manual de Vendas 05) são exibidos como checklist na troca de etapa; a lista de insucessos vira alerta automático.
- **RN-2.5:** o modelo de negócio (5 modelos da emenda 1.1a) acompanha a oportunidade e alimenta ciclo médio e forecast (M5).

## 3. Decisões do owner (formulário 12/11 21:48)

1. Emenda 1.2a RATIFICADA (campos do lead e da oportunidade).
2. Pipeline de **duas trilhas pela complexidade** (esta emenda).
3. **Valor estimado OBRIGATÓRIO** na oportunidade (exigido para avançar de etapa).
4. Pendências técnicas da Fase 1 entram na Fase 2 (endpoint de importação em lote no servidor + categorização da origem "LP" nos contadores).
5. **Pipeline agora, IA depois** — emenda 1.3a (IA nativa pré-vendas) aguarda ratificação com a consultora + 10 casos no casos.csv.
