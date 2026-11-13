# Briefing — Fase 2, Bloco 1 (F2-T01 a F2-T04 + F2-T08/T09 pendências técnicas)

**Data:** 2026-11-12 · **Autor:** Bart (revisão/provas) · **Para:** Claude Code (implementação)
**Decisões do owner (12/11 21:48):** emenda 1.2a RATIFICADA · duas trilhas pela complexidade · valor estimado OBRIGATÓRIO · pendências técnicas incluídas · pipeline agora, IA depois.

## Contexto

Fase 1 fechada (23/23). Fase 2 = M2 (Oportunidade & Pipeline). **Correção do owner:** o pipeline NÃO é uma lista única — a trilha de etapas segue a complexidade do produto (emenda 1.2b): **Baixa → AHM Cycle (5 fases)**; **Alta → Site Assessment (8 etapas)**. Fonte canônica: emenda 1.1a §4. O produto já tem `complexidade` ("Baixa"/"Alta") gravado no catálogo.

## Tasks deste bloco

### F2-T01 — Campos de entrada no lead (emenda 1.2a §3)
- Novos campos no lead: `area_interesse` (select: Qualidade no Transporte | Cadeia do Frio | Segurança e Logística), `pais` (text, default Brasil), `idioma` (select: pt-BR | es | en, default pt-BR), `modelo_compra` (select: Nacional | Importação).
- Formulário Novo/Editar Lead + colunas opcionais na importação CSV (mesmo padrão do campo telefone do Bloco 4: HEADER_MAP tolerante).
- Ao qualificar, `area_interesse` sugere famílias candidatas (Cadeia do Frio → família 2; Segurança e Logística → 4/4a/5; Qualidade no Transporte → 1/3/6/7) — sugestão, não bloqueio.
- Migração: campos novos na coleção leads (sem quebrar os 15.370 existentes — todos nulos/vazios).

### F2-T02 — Campos obrigatórios da oportunidade (emenda 1.2a §3 + decisão 3)
- Novos campos na oportunidade: `ambiente_fechamento` (select: Venda em reais | Locação em reais | Exportação (EUA)), `moeda` (BRL|USD, derivado do ambiente), `entidade_vendedora` (AHM Brasil | AHM Solution Americas, derivado do ambiente), `valor_estimado` (number, OBRIGATÓRIO — decisão do owner).
- Derivação automática: Venda em reais → BRL/AHM Brasil; Locação em reais → BRL/AHM Brasil; Exportação (EUA) → USD/AHM Solution Americas. Campos derivados ficam automáticos (se o owner quiser editar depois, vira emenda).
- Oportunidade NÃO avança de etapa sem os 4 campos preenchidos (validação na troca de etapa + no fechamento ganho/perda).
- Modal da oportunidade: seção nova "Fechamento" com os 4 campos.

### F2-T03 — Trilhas de pipeline por complexidade (emenda 1.2b)
- Coleção nova `pipeline_etapas` (seed das 13 etapas): campo trilha (baixa/alta), ordem, nome, fase do fluxo (Pré-venda/Venda/Pós-venda), taxa_alvo (60/70/80/90/95/100/100/100 p/ alta; 60/70/90/100/100 p/ baixa — mapeando AHM Cycle 1-2≈1-2, 3≈3-5, 4≈6, 5≈7-8), complexidade.
  - Trilha baixa (AHM Cycle): 1. Problema Emergente · 2. Diagnóstico Técnico Compartilhado · 3. Proposta Customizada · 4. Implementação Piloto · 5. Acompanhamento/Expansão.
  - Trilha alta (Site Assessment): 1. Cenário Atual · 2. Diagnóstico (MQL→SQL) · 3. Refinamento (visita técnica) · 4. Apresentação de Projeto · 5. Negociação (PoC) · 6. Implementação · 7. Realidade · 8. Adoção Corporativa.
- Oportunidade recebe `etapa_atual` (relation para pipeline_etapas) + `trilha` (baixa/alta, derivada da complexidade do produto — RN-2.1, não muda manualmente).
- Tela da oportunidade mostra a trilha como stepper (5 fases ou 8 etapas conforme a trilha) com a etapa atual destacada.
- Oportunidades existentes (2 reais): sem produto com complexidade → trilha indefinida até escolherem produto; stepper mostra estado "trilha não definida" sem erro.
- Hook: ao criar oportunidade a partir de lead qualificado, trilha/etapa inicial = primeira etapa da trilha do produto (se o lead tem produto com complexidade).

### F2-T04 — Passagem de etapa com data e histórico (RN-2.2)
- Coleção nova `oportunidade_etapas` (histórico): oportunidade, etapa, entrou_em (data), saiu_em (data), responsável.
- Trocar de etapa grava: saiu_em na etapa atual, novo registro com entrou_em=agora. Voltar de etapa também grava (histórico completo, sem apagar).
- Modal da oportunidade: histórico visível (lista cronológica com datas e responsável).
- Validação F2-T02 aplicada: sem ambiente/moeda/entidade/valor → troca de etapa bloqueada com mensagem corporativa.

## Pendências técnicas (F2-T08, F2-T09) — podem entrar no mesmo bloco ou no seguinte

### F2-T08 — Endpoint de criação em lote no servidor
- Endpoint POST /api/leads/batch (auth-only): recebe array de leads, valida, cria em transação/lote. Meta: 7.500 leads < 1 min (hoje ~3 min via criações paralelas do navegador).
- Importação do navegador passa a usar o endpoint quando disponível; fallback = caminho atual (10 paralelas).
- Proteções do Bloco 5 mantidas: verificação de duplicados ANTES (chaves contato+empresa e nome+empresa), bloqueio em caso de falha na verificação, relatório final.

### F2-T09 — Categorização da origem "LP"
- Lead 5h8bnolgb8ndhkm (Aroldo) tem origem "LP" e conta em "todas" mas não em manual/csv/demo — soma dos contadores ≠ total.
- Categorizar origem LP no painel (decidir: LP = Landing Page → conta como captura web; ou nova fatia). Proposta: fatia própria "Landing Page" nos contadores do K1/painel.

## Regras transversais

- Linguagem pt-BR corporativo em toda string de UI (sem códigos internos RN-x.x/EM-x.x/F2-Txx).
- Migrações com 4 dígitos sequenciais (0024+).
- Hooks PocketBase: usar hooks de REQUISIÇÃO (onRecordCreateRequest etc.) quando precisar de auth/validação de payload; model hooks não têm e.auth.
- Base real intacta: 15.370 leads, Eduardo Zuin (Aprovada), Aroldo (Rejeitada, 3 interações), 2 transbordos, 2 oportunidades.
- Testes automáticos na pasta de testes (QA do Skip não executa — rodar à parte e relatar).
- Provas do Bart depois: campos no formulário + importação; bloqueio de etapa sem campos; duas trilhas no stepper; histórico com datas; endpoint em lote medido; contadores LP.
