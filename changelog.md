# Changelog — CRM Atendimento AHM

## 0.0.78 (24/11/2026) — Bloco R1 · datas reais (R6, emenda 1.4a)
- **APROVADO PELO BART — 32 provas de API + 6 provas de tela, todas verdes; CONCLUÍDO com autorização do owner ("pode concluir", 24/11 15:23)**
- **ADENDO 1 (owner, 24/11 16:10)**: o "pode concluir" de 24/11 15:23 vale como APROVAÇÃO TÉCNICA. Pela carta (seção 8), a conclusão formal sai após o teste humano — a Rose refaz o caso real com as datas reais; o Bloco R1 segue "pronto tecnicamente, aguardando teste humano".
- Decisão do owner: toda data guarda o momento REAL do fato, com a data do lançamento registrada à parte; as métricas passam a usar a data real
- Migration 0038: `leads.entrada_em` (backfill: data original do Pipedrive, senão created) · `oportunidades.inicio_ocorrido_em` e `resultado_ocorrido_em` (campos de entrada, nunca persistem) · `oportunidade_fechamentos.ocorrido_em`
- Hook on_leads_entrada.js: entrada sem data → agora; data informada → não futura (dia de Brasília); correção permitida na alteração
- Hook on_oportunidade_pipeline.js: início informado na criação → vira `criado_em` + `etapa_desde` (não futuro, não anterior à entrada do lead); correção do início só na 1ª etapa, sem resultado, em alteração separada; troca de etapa com data real → histórico com `data_informada`
- Hook on_oportunidade_fechamento.js: ganho/perda com data real → `fechado_em` (não futura, não anterior ao início ou à etapa atual); histórico grava `ocorrido_em` (data real) × `registrado_em` (data do lançamento)
- Métricas (Desempenho) usam a data real: oportunidade com início no mês passado NÃO conta no período "mês" (provado: contagem inalterada)
- Tela: campo "Data em que a etapa foi concluída" na conclusão (padrão hoje); histórico mostra "Entrou em DD/MM/AAAA (data informada; lançado em DD/MM/AAAA, HH:MM)"
- Provas do Bart: entrada retroativa/futura/sem data; início retroativo/futura/antes do lead/correção separada/após avançar; troca de etapa com data real (histórico e saiu_em); ganho/perda com data real (fechado_em, histórico, futura, anterior); D5 métricas; tela: caso da Rose completo com datas reais, selos na lista, Minha mesa limpa, conclusão com data passada e histórico na tela
- Limpeza: todos os registros PROVA apagados; base 15.406 intacta

## 0.0.77 (24/11/2026) — Bloco R1 · correção A1
- **APROVADO PELO BART — re-prova 6/6; parte da conclusão do Bloco R1**
- A1 (achado da revisão): `etapa_ocorrido_em` enviado na criação persistia e contaminava updates futuros (todo avanço falhava com "data anterior à etapa anterior")
- Correção: hook de criação descarta o campo; hook de update só considera a data se ela vier NESTA requisição (diferir do valor gravado) — valor antigo é ignorado e limpo

## 0.0.76 (24/11/2026) — Bloco R1 · fundação do padrão + bugs da Rose
- **APROVADO PELO BART — 30 provas de API + tela; relatório 06_notas/revisao-bloco-r1-provas-bart.md**
- R1.1 (B1/B2): `leads.situacao_comercial` derivada no servidor (sem oportunidade · em negociação · ganho · perdido), recalculada após criar/alterar/excluir oportunidade por qualquer origem; prioridade aberta > ganha > perdida; rejeitadas não contam; gravação por SQL direto; backfill de todos os leads; Minha mesa só traz lead sem oportunidade; lista de leads com selo "Em negociação"/"Venda ganha"/"Venda perdida"
- R1.3 (B3): troca de produto no lead → oportunidades abertas mudam de produto com nota no comentário; fechadas não mudam; trilha recalculada (RN-2.1)
- R1.4 (B4): `etapa_ocorrido_em` — data real da troca de etapa (futura → 400; anterior à etapa anterior → 400; sem data → agora); histórico com `data_informada`
- Regressão verde: fechamento, bloqueio de etapa, permissões, proteção PROVA/DEMO
- Migration 0037 (a 0036 fica reservada ao roteamento por família)

## 0.0.74 (14/11/2026) — F0.1–F0.5 da IA de vendas: fundação
- **APROVADO PELO BART** (31/31 provas re-executadas em 24/11, exceto roteamento — em revisão para família); relatório 06_notas/revisao-f0-ia-provas-bart.md
- Migration 0035: base_conhecimento (fonte obrigatória), regras_roteamento (Assinatura/Cíclico→Julio ativas; Pontual→Paulo inativa aguardando cadastro; Locação/Consultoria sem regra → fila do admin), janelas_executivo, ia_envios (só o sistema grava — nem admin), configuracoes.ia_modo=rascunho
- Hook ia_fundacao.js: permissões (CRUD das 3 coleções de cadastro só admin; base/ia_envios leitura só admin; ia_envios escrita SÓ pelo servidor), validações (fonte obrigatória, dia_semana 0-6, início<fim, regra ativa exige vendedor, ia_modo ∈ {desligada,rascunho,automatica}), rota GET /backend/v1/ia/horarios-previa (só admin; agenda não conectada → sempre vazio c/ erro agenda_indisponivel — nenhum horário inventado)
- Tela /ia (só admin): modo de operação, fila de envios, roteamento, janelas, base de conhecimento
- **ROTEAMENTO EM REVISÃO (decisão do owner)**: por FAMÍLIA DE PRODUTO (a dor da 1ª conversa aponta para a família; modelo de negócio só na proposta), c/ área de interesse como reserva; sem identificação → fila do admin. Migration 0036 após autorização; seed sai das 4 planilhas do owner (05_dados/ia-vendas/ — NUNCA ao GitHub)

## 0.0.73 (14/11/2026) — Bloco D: importação do Pipedrive
- **APROVADO PELO BART** (provas a-f verdes na 1ª rodada); **SUSPENSO pelo owner** — histórico do Pipedrive não é confiável; NADA importado; rota/tela ficam prontas para reavaliação
- Migration 0034 (campos pipedrive_*), rota POST /backend/v1/pipedrive/importar (só admin; simular=true não grava; lotes de 500; dedup por pipedrive_id + empresa+pessoa), tela /importar-pipedrive

## 0.0.72 (13/11/2026) — Bloco C: desempenho e relatórios
- **APROVADO PELO BART** (relatório 06_notas/revisao-bloco-c-provas-bart.md): rota /backend/v1/desempenho provada c/ login; provas (a)-(f) verdes; D1 corrigido; A3 resolvido (sem frase inventada)
- Tela Desempenho, Minha mesa na rota /, Responsável herdado do lead, 25 motivos de perda + "Outro (especificar)" inativo, meta 20/mês, moedas R$/US$ separadas, Tickets/Atendimentos fora do menu
- Migration 0033

## 0.0.69 (13/11/2026) — Bloco B: telas grandes
- **APROVADO PELO BART** (32/32 provas nos Blocos A+B): rota /oportunidades/:id em tela cheia, bloqueio de etapa sem fechamento → 400, fontes ≥13px, "Manter-me conectado" funcional

## 0.0.68 (13/11/2026) — Bloco A/UX-A: segurança e estabilidade
- **APROVADO PELO BART** (24/24 provas): login sem credenciais expostas, "Solicitar acesso" sem contatos inventados, migration 0032 (usuários logados veem colegas), papel no header, sem "v1.0.0"

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **APROVADO PELO BART** (23/24 provas; D3 era falso positivo do script do Bart; D4 menor)
- Migration 0031 (users.papel admin|vendedor), regras de users, hook permissoes_papel.js, tela Equipe só admin

## 0.0.62–0.0.64 (13/11/2026) — Bloco 2: proteção, checklist, fechamento, taxas
- **APROVADO PELO BART** (API + tela): proteção contra exclusão (só PROVA/DEMO; real → 403 c/ sugestão de Arquivar; arquivamento reversível; exclusoes_auditoria), checklist por etapa (imutáveis; insucesso → alerta), fechamento ganho/perda (motivo+concorrente; exige ambiente+valor>0; troca direta → 400; reabertura c/ justificativa; oportunidade_fechamentos só servidor), painel de taxas real × alvo + win rate (meta 40%), cartão "Próximo passo"
- Migrations 0027-0030 (0030 = seed do checklist, 67 itens conferidos por API)

## 0.0.60–0.0.61 (12/11/2026) — Bloco 1: entrada e pipeline
- **APROVADO PELO BART** (8 provas): campos de entrada no lead, fechamento (moeda/entidade derivadas no servidor), 2 trilhas (13 etapas; RN-2.1; 1ª etapa automática), histórico de etapas, POST /backend/v1/leads/lote (1.000 leads em 0,4s), origem LP nas métricas
- Migrations 0024-0026

## 0.0.57–0.0.59 (12–13/11/2026) — Blocos 4 e 5 (Fase 1)
- Bloco 5 APROVADO (D1: reconhecimento por nome+empresa; D2: varredura determinística c/ retry e conferência de cobertura)
- Bloco 4 APROVADO (duplicidade contra a base inteira na prévia; telefone na importação; correção 0.0.57 plural pt-BR)
- Incidente 12/11: reimportação criou 7.548 duplicados — limpos c/ verificação por registro; base final 15.370

## 0.0.56 e anteriores (Fase 1)
- Fase 1 CONCLUÍDA 23/23 (13/11, teste do Bloco 5 aprovado pelo owner). Ver histórico no git.

## 2026-09-17
- Pasta operacional inicial criada para a Fase 1.
