# EMENDA 1.2a — CRM próprio AHM, arquitetura de plataformas e campos de entrada do lead

**Data:** 2026-11-10
**Origem do sinal:** Afonso Moreira (owner AHM Solution) — sessão de trabalho com o Ethos em 10/11/2026: decisão de desenvolver CRM próprio em vez de Pipedrive, revelação do ERP próprio em construção (Claude Code), envio dos mapas de processo Mkt/Vendas e ADM/FIN/COMEX/LOG, e respostas às três perguntas abertas do mapeamento.
**Status:** RATIFICADA pelo owner em 12/11/2026 (21:48, formulário de decisões da Fase 2) — campos novos entram no briefing da Fase 2
**SPECs afetadas:** SPEC 1.2 (leads — campos de entrada), SPEC 1.5 (fila de oportunidades — ambiente de fechamento), SPEC 1.6 (KPIs — previsão), SPEC 1.7 (interações — timeline)
**Fontes:** mapa Mkt/Vendas (`artifacts/mapeamento-fluxo-mkt-vendas-ahm.md`), mapa ADM/FIN/COMEX/LOG (`artifacts/mapeamento-fluxo-adm-fin-comex-log-ahm.md`), relato do owner sobre estrutura BR/EUA e ERP próprio

---

## 1. Decisão de produto: CRM próprio AHM (substitui Pipedrive como CRM central)

O owner decidiu construir o CRM próprio em vez de adotar/manter Pipedrive como CRM central.

**Regra de construção:** copiar simples o que é commodity (pipeline, atividades, listas, busca); inventar fundo o que é diferencial da AHM (metodologia: famílias de diagnóstico, BANT, Site Assessment auditável, AHM Cycle com recompra PUSH). Integração nativa com OMIE — não via middleware.

**Roadmap de módulos (faseado por necessidade e prioridade):**

| Módulo | Conteúdo | Fase |
|---|---|---|
| M1 — Leads & Qualificação | leads manuais/CSV, duplicidade, estados de qualificação, transbordo, diagnóstico por família | **Fase 1 (concluída 13/11/2026 — 23/23)** |
| M2 — Oportunidade & Pipeline | oportunidade a partir de lead qualificado, decisões aprovar/rejeitar/revisar, proposta, ambiente de fechamento, win rate | **Fase 2 (em curso)** |
| M3 — Pós-venda | AHM Cycle e Site Assessment auditáveis por etapa, timeline de interações, recompra PUSH | Fase 2/3 |
| M4 — Integrações | webhook RD Station (A1), Agente IA WhatsApp (A2), OMIE (faturamento/recorrência), QuickBooks | Fase 3 |
| M5 — Inteligência | KPIs K1/K2/K3, painel de conversão por etapa, forecast por modelo de negócio | Fase 3/4 |

**Fora de escopo desta emenda:** substituir o ERP próprio do owner, ou definir a integração técnica CRM↔ERP (fica para o módulo M4).

---

## 2. Arquitetura de plataformas da AHM (fronteira entre sistemas)

| Sistema | Domínio | Responsabilidade |
|---|---|---|
| **CRM — Adapta Native** (este projeto) | Relacional / comercial | lead → qualificação → oportunidade → ganho → pós-venda → recompra |
| **ERP — torre de controle** (Claude Code, owner) | Transacional / operacional | simulação de custo e pricing (importação, exportação, revenda, serviços), aprovações por permissão, processos de importação e status, dashboards, IA de previsão de vendas e sazonalidade |
| **OMIE** | ERP Brasil | cadastro, NF, contas a receber, conciliação, faturamento de serviços |
| **QuickBooks** | Gestão EUA | cadastro e faturamento da operação de exportação (AHM Solution Americas) |
| **RD Station / Pipedrive / Envya** | Captação e atendimento (legado) | RD Station mantém captura e marcação de venda; Pipedrive sai como CRM de destino; Envya é o BOT de atendimento |

**Ponto de integração natural CRM→ERP:** oportunidade **ganha** no CRM + pedido de compra recebido → ERP assume (simulação/pricing → aprovação → importação/entrega) → status de entrega volta para a timeline do CRM.

**Dados que o CRM fornece ao ERP (para a IA de previsão de vendas):** leads por etapa, conversão por etapa, win rate, ciclo médio por modelo de negócio, oportunidades abertas com valor e ambiente de fechamento.

---

## 3. Campos novos no lead (SPEC 1.2) — entrada e ambiente de fechamento

O mapa de processos mostra que o cadastro já bifurca hoje: **NACIONAL → OMIE**, **IMPORTAÇÃO → QUICKBOOKS**. E o owner confirmou que leads internacionais entram no mesmo funil.

| Campo novo | Tipo | Valores / origem | Regra |
|---|---|---|---|
| `area_interesse` | select | Qualidade no Transporte · Cadeia do Frio · Segurança e Logística | Recorte de ENTRADA (marketing), distinto de `familia_tecnologica` (recorte TÉCNICO de diagnóstico). Sugere a família ao qualificar. |
| `pais` | text | ISO ou nome | Default Brasil; obrigatório para lead internacional |
| `idioma` | select | pt-BR · es · en | Default pt-BR |
| `modelo_compra` | select | Nacional · Importação | Decisão do LEAD no refinamento; define sistema de cadastro (OMIE × QuickBooks) |
| `ambiente_fechamento` | select | Venda em reais · Locação em reais · Exportação (EUA) | Decidido após qualificação, quando produto/modelo tem aderência — alimenta M2 |
| `moeda` | select | BRL · USD | Derivado do ambiente de fechamento |
| `entidade_vendedora` | select | AHM Brasil · AHM Solution Americas | Derivado do ambiente de fechamento |

**Nota de nomenclatura:** `modelo_compra` (Nacional/Importação) é decisão de **como a mercadoria chega**; `ambiente_fechamento` é decisão de **onde o negócio é fechado** (moeda e entidade). São campos distintos — o mapa ADM usa o primeiro, o relato do owner usa o segundo.

**Impacto nas tasks:** os campos entram na F1-T05 (importação CSV) como colunas opcionais do arquivo e no formulário de lead; a obrigatoriedade plena é do M2.

---

## 4. Áreas de interesse × família tecnológica

- **Área de interesse** = porta de entrada do lead (marketing, 3 opções do mapa).
- **Família tecnológica** = recorte técnico do diagnóstico (8 famílias, emenda 1.1a).
- **Relação:** a área sugere famílias candidatas (ex.: Cadeia do Frio → família 2; Segurança e Logística → famílias 4/4a/5), mas a família final é definida pelo **produto de interesse** escolhido.

---

## 5. Modelos de negócio × trilhas do mapa

O mapa Mkt/Vendas mostra 3 trilhas (Cíclica, Assinatura, Pontual). Os 5 modelos da emenda 1.1a mapeiam assim (confirmado pelo owner em 10/11):

| Modelo (emenda 1.1a) | Trilha do mapa |
|---|---|
| Cíclico | Venda Cíclica |
| Assinatura | Assinaturas |
| **Locação** | **Assinaturas** (condições da assinatura, pagamento em 30 dias) |
| Pontual | Venda Pontual |
| **Consultoria de Serviços** | **Venda Pontual** (projetos personalizados) |

---

## 6. Impacto nas tasks da Fase 1

- **F1-T04 (concluída)** — permanece válida; os campos novos são **aditivos** (nenhum campo existente muda de significado).
- **F1-T05 (importação CSV)** — passa a aceitar as colunas novas como opcionais; o recorte obrigatório continua `nome, empresa, contato`.
- **F1-T06 (duplicidade)** — sem impacto.
- **F1-T07 em diante** — a qualificação pode usar `area_interesse` para sugerir família.
- **M2 (Fase 2)** — `ambiente_fechamento`, `moeda` e `entidade_vendedora` viram campos obrigatórios da oportunidade.

---

## 7. Pendências de decisão

1. ~~**Ratificação desta emenda** pelo owner~~ — **RATIFICADA 12/11/2026** (formulário de decisões da Fase 2).
2. **Frota de locação**: o owner não sabe onde é registrada hoje ("deveria estar no OMIE"). Decidir se vira módulo do ERP (transacional: equipamento, contrato, devolução, calibração) com visão no CRM, ou coleção própria no CRM.
3. **Contrato de integração CRM↔ERP**: formato dos dados e eventos (definir no M4, quando o ERP estiver mais maduro).
4. **Pipedrive**: confirmar se sai de operação imediatamente ou se permanece em paralelo durante a transição (impacto: dupla digitação no time de vendas).
