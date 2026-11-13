# Fase 2 — Oportunidade & Pipeline (M2)

**Regra da fase:** nenhuma task depende de API, credencial, webhook, conector, integração externa ou decisão pendente do cliente.

**Decisões do owner (12/11 21:48):** emenda 1.2a RATIFICADA · pipeline de duas trilhas pela complexidade (emenda 1.2b) · valor estimado obrigatório · pendências técnicas da Fase 1 incluídas · pipeline agora, IA depois (emenda 1.3a aguarda consultora + 10 casos).

## Tasks

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F2-T01 | Campos de entrada no lead (emenda 1.2a): área de interesse, país, idioma, modelo de compra | Ethos | SPEC 1.2 (EM 1.2a) | Campos gravam no lead manual e na importação CSV (colunas opcionais); área sugere família ao qualificar | GREEN+REGRESSÃO | captura do formulário e da importação | Fase 1 🔶 |
| F2-T02 | Campos obrigatórios da oportunidade (emenda 1.2a): ambiente de fechamento, moeda, entidade vendedora + valor estimado obrigatório | Ethos | SPEC 1.5 (EM 1.2a) | Oportunidade não avança de etapa sem ambiente/moeda/entidade/valor | erro+REGRESSÃO | captura do bloqueio | F2-T01 🔶 |
| F2-T03 | Trilhas de pipeline por complexidade (emenda 1.2b): AHM Cycle 5 fases (baixa) × Site Assessment 8 etapas (alta) | Ethos | SPEC 1.8 (EM 1.2b) | Oportunidade baixa mostra 5 fases; alta mostra 8 etapas; trilha definida pelo produto | GREEN+REGRESSÃO | captura das duas trilhas | F2-T02 🔶 |
| F2-T04 | Registro de passagem de etapa com data e histórico (RN-2.2) | Ethos | SPEC 1.8 (EM 1.2b) | Troca de etapa grava data; histórico completo visível na oportunidade | GREEN+REGRESSÃO | captura do histórico | F2-T03 🔶 |
| F2-T05 | Checklist de critérios de conclusão e alerta de insucessos na troca de etapa (RN-2.4) | Ethos | SPEC 1.8 (EM 1.2b) | Checklist por etapa exibido; insucessos geram alerta | GREEN | captura do checklist | F2-T04 | ⬜ |
| F2-T06 | Proposta e fechamento (ganho/perda) com motivo auditável | Ethos | SPEC 1.8 | Ganho/perda exige motivo; auditoria completa | GREEN+REGRESSÃO | captura do fechamento | F2-T04 | ⬜ |
| F2-T07 | Painel de pipeline: taxas de passagem real × alvo por etapa (RN-2.3) | Ethos | SPEC 1.8 (EM 1.2b) | Painel compara real × alvo das duas trilhas; estado sem dados | REGRESSÃO | captura do painel | F2-T04 | ⬜ |
| F2-T08 | Endpoint de criação em lote no servidor (pendência Fase 1 — desempenho da importação) | Ethos | SPEC 1.2 (emenda) | 7.500 leads < 1 min | medição | log de tempo | Fase 1 🔶 |
| F2-T09 | Categorização da origem "LP" nos contadores do painel (pendência Fase 1) | Ethos | SPEC 1.6 | Origem LP conta corretamente em todas as fatias | REGRESSÃO | captura dos contadores | Fase 1 🔶 |
