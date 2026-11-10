# EMENDA 1.1a — Modelos de negócio, templates de diagnóstico por família e Site Assessment no catálogo

**Data:** 2026-11-09
**Origem do sinal:** Afonso Moreira (owner AHM Solution) — consolidação da metodologia comercial em sessão de trabalho com o Ethos (modelos de negócio, matriz de complexidade, Site Assessment, perguntas de reflexão e templates de diagnóstico por família)
**Status:** RATIFICADA — 2026-11-09 22:58 BRT, aprovação do owner Afonso Moreira ("vamos ratificar") como decisão de produto; consultora informada via changelog
**SPEC afetada:** SPEC-1-001 (catálogo de produtos); reflexos em SPEC 1.2 (leads), 1.3 (qualificação), 1.4 (transbordo), 1.6 (KPIs), 1.7 (interações)
**Fontes:** slides de modelos de negócio (4), Playbook de Vendas Baixa Complexidade (16/04/2026), Bíblia de Vendas/Site Assessment (21/04/2026), Site Assessment detalhado (20p), Manual de Vendas 05 (2020, perguntas de reflexão), 8 templates de diagnóstico (14 documentos), definições verbais do owner (registradas na sessão)

---

## 1. Coleção nova `modelos_negocio` (substitui o enum tipo_venda)

O campo `tipo_venda: Recorrente|Pontual` (F1-T01) não comporta a realidade comercial. Criar coleção de referência com 5 modelos e vincular o produto a ela:

| Modelo | Ciclo | Pagamento | Canal | Fornecedores | Previsibilidade | Complexidade típica |
|---|---|---|---|---|---|---|
| Assinatura | 30–60 dias | Faturado 15 dd | Direto/digital | — | Alta | Baixa |
| Locação | Condições da assinatura | Pagamento em 30 dias | Direto/digital | — | Alta | Baixa |
| Cíclico | 45–120 dias | Faturado 21 dd (negociável) | Direto/digital | SpotSee | Alta (PUSH de recompra) | Baixa (maioria) |
| Pontual | ~180 dias | 100% antecipado OU 50/50 OU 21 dd | Direto/digital | Meijer, Seen Safety, Frederick Energy, Proxicam, Orlaco | Não garante | Alta |
| Consultoria de Serviços | 60–90 dias | Faturado 30 dd | Direto/digital | A própria AHM | — | Alta |

**Campos sugeridos:** nome, ciclo_dias_min, ciclo_dias_max, condicoes_pagamento, canal, fornecedores, previsibilidade (bool), descricao.
**Nota:** a Bíblia de Vendas (abr/2026) descreve "Venda Cíclica" como 6–12 meses/30-60-90 — **desatualizada**; definição vigente é a da tabela acima (confirmada pelo owner). "Instalações" não é oferecida hoje (retoma em alguns meses) — fora do escopo desta emenda.

## 2. Matriz produto × complexidade × família tecnológica

Campo novo `familia_tecnologica` no produto (relation para a família do template de diagnóstico):

- **Baixa complexidade (8):** ShockWatch MAG2000, TiltWatch/Plus, HI Cards, Thermax, WarmMark QR, FreezeSafe QR, Farol Blue Spotlight, Red Light (Red Zone) — ROI 50–400%, ciclo 1–4 meses, modelos cíclico/assinatura/locação.
- **Alta complexidade (12):** SpotBot 4G, ShockLog 298, Push-Pull, Data Loggers, Hit-Not, Iris860, Iris-i, Proxicam, Câmera Empilhadeira Sem-Fio, Orlaco/Motec, Hit Not Report, Trocador de Paletes — ROI 150–1000%, ciclo 2–15 meses, modelos pontual/consultoria.

## 3. Coleção nova `templates_diagnostico` (um template por FAMÍLIA, não por produto)

8 famílias mapeadas a partir de 14 documentos-fonte. Estruturas modulares: blocos comuns + blocos específicos por família.

| # | Família | Produtos | Estrutura | Fonte |
|---|---|---|---|---|
| 1 | Impacto/tombamento descartável | Shockwatch Label, Shockwatch 2, TiltWatch Plus, XTR, MAG2000 | 22 perguntas técnicas | Diagnóstico Shockwatch® |
| 2 | Cadeia do frio / indicadores | WarmMark QR, FreezeSafe QR, HI Cards | 11 perguntas | Qualificação de site – Monitoramento |
| 3 | Registradores de impacto | ShockLog 298, SpotBot 4G, Data Loggers | 12 técnicas + 5 cenário | Diagnóstico Shocklog + Cenário Atual |
| 4 | Segurança/proximidade | Hit-Not, Iris860, Iris-i, Proxicam, Farol Blue, Red Light | 24 técnicas + 4 cenário | Diagnóstico HIT-NOT® + Cenário Atual Hit Not/Iris |
| 4a | Assessoria de plano de tráfego (serviço) | — (linha Consultoria) | 17 perguntas auditoria ISO (CUMPRE + observações) + checklist autoavaliação 6 seções | Diagnóstico Segurança Viária + Checklist AHM |
| 5 | Visibilidade/câmeras | Câmera Sem-Fio, Orlaco, Motec | 11 perguntas | Diagnóstico – Câmera |
| 6 | Palletless | Push-Pull, RollerForks | base 14p/16p + 5 variantes por tipo de carga (Big Bags 7p, Chapas 6+6p, Fardo 6p, Sacaria manual 15p, Sacaria c/palete 20p) | 7 documentos |
| 7 | Trocador/Transferidor de paletes | Trocador de Paletes | 10 perguntas | Diagnóstico Transferidor de Paletes (2021) |
| 8 | Eficiência industrial | Thermax | BANT completo: 25 base + 23 por vertical (Saúde/CME, Indústria, Alimentos/HACCP, Têxtil) + sinais verdes/vermelhos + matriz de qualificação | Assessment AHM — Roteiro de Qualificação Consultiva |

**Tipos de template:** `tecnico` (objetivas), `cenario` (reflexão comercial SPIN-like), `auditoria` (ISO CUMPRE+obs), `checklist_triagem`, `qualificacao_consultiva` (BANT + verticais + sinais).
**Blocos comuns entre famílias:** identificação do lead, projeto vs sondagem, claims (registro/departamento/contato), apontamento visual vs gravação (escala 1–10), contexto (armazém vs transporte).
**Blocos específicos:** dimensionamento logístico (impacto), infraestrutura de monitoramento (frio), G-force/fixação/valor (registradores), frota/locador/torre (câmeras), núcleo palletless (dimensões, fotos, fornecedor slipsheets, custo/hora/homem, caminhões/dia, embarques/mês, estudo de otimização), BANT + verticais (Thermax).

**Regras de negócio novas:**
- **RN-1.4:** todo produto referencia o template de diagnóstico da sua família tecnológica.
- **RN-1.5 (regra de ouro Thermax):** produto não é apresentado antes do diagnóstico completo — produto cedo demais transforma assessment consultivo em negociação de preço.
- **RN-1.6:** sinais vermelhos (Thermax: quer só preço / sem processo térmico definido / sem responsável pela qualidade / não aceita diagnóstico) desqualificam a oportunidade.

## 4. Site Assessment como fluxo auditável no sistema

8 etapas (nomenclatura ATUAL) × 3 fases, com objetivo, responsável, duração, taxa de passagem, critérios de conclusão e perguntas de reflexão (Manual de Vendas 05, consolidado e aprovado pelo owner):

| Fase | Etapa | Responsável | Duração | Taxa alvo |
|---|---|---|---|---|
| Pré-venda | 1. Cenário Atual | Analista Comercial Jr. | 1–3 d | 60% |
| Pré-venda | 2. Diagnóstico (MQL→SQL) | Analista Comercial Pl/Sr + Especialista de Categoria | 1–3 d | 70% |
| Pré-venda | 3. Refinamento (visita técnica) | Analista Comercial Pl/Sr | 1–15 d | 80% |
| Venda | 4. Apresentação de Projeto | KAM/Coordenador | 1–15 d | 90% |
| Venda | 5. Negociação (PoC) | KAM/Coordenador | 1–20 d | 95% |
| Venda | 6. Implementação | Equipe Técnica/Engenheiros | 1–30 d | 100% |
| Pós-venda | 7. Realidade | Qualidade e Relacionamento | 1–30 d | 100% |
| Pós-venda | 8. Adoção Corporativa | Consultor + Marketing | 1–30 d | 100% |

- **AHM Cycle (baixa complexidade):** 5 fases (Problema Emergente → Diagnóstico Técnico Compartilhado → Proposta Customizada → Implementação Piloto → Acompanhamento/Expansão) mapeiam sobre as etapas 1-2 / 3-5 / 6 / 7-8 + gatilho do próximo ciclo (PUSH de recompra 45–120 dias).
- **Metodologias de apoio:** ABCDT e SPIN (etapa 2); MAPAN/ZOPA e 4 estágios da negociação (etapa 5).
- **Regra transversal:** "documentar tudo no CRM" — o sistema é essa documentação estruturada.
- **Objetivo declarado pelo owner:** garantir entendimento e controle do processo de vendas — perguntas de reflexão como checklist por etapa, fatores de sucesso como critérios de conclusão, lista de insucessos como alerta.

## 5. Impacto nas tasks da Fase 1

- **F1-T03 (fixture)** — redefinida por esta emenda: produto exemplo Shockwatch completo (especialista vinculado, timing, FAQ) + template da família impacto/tombamento carregado + vínculo produto↔template. Liberada após ratificação.
- **Tasks candidatas pós-ratificação (não criadas ainda):** migração `modelos_negocio` + seed dos 5 modelos; migração `templates_diagnostico` + seed das 8 famílias; campo `familia_tecnologica` + relation produto↔template; extensão do tipo_venda; fluxo de etapas do Site Assessment no lead (reflexo SPEC 1.7).
- **F1-T01 e F1-T02 concluídas permanecem válidas** — a emenda estende, não desfaz (o enum tipo_venda será substituído por relation; hooks e tela seguem funcionando).

## 6. Pendências de decisão

1. ~~Ratificação da consultora~~ — RATIFICADA pelo owner em 2026-11-09 22:58 BRT como decisão de produto.
2. Definição de onde o Site Assessment vive: coleção própria `etapas_oportunidade` vs. campos no lead (decisão de arquitetura na task).
3. Hit Not Report (plataforma de dados) — sem template próprio recebido; confirmar se usa o template da família segurança ou tem questionário próprio.
4. Usuário "Especialista Produto" (fixture) — criar como usuário de demonstração ou aguardar cadastro real da equipe.
