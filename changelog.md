# Changelog — CRM Atendimento AHM

## 0.0.74 (14/11/2026) — F0.1–F0.5 da IA de vendas: fundação
- **ROTEAMENTO EM REVISÃO (decisão do owner 14/11)**: rotear por FAMÍLIA DE PRODUTO, não mais por modelo de negócio — a dor da 1ª conversa aponta para a família; o modelo de negócio só é decidido na proposta. Desenho: regra por família c/ área de interesse como reserva; sem identificação segura → fila do administrador. Planilha do owner em 05_dados/ia-vendas/ (NÃO vai ao GitHub). Migration 0036 DEPOIS da autorização do owner. Provas da F0 re-executadas pelo Bart EXCETO o critério da tabela de roteamento: 31/31 PASS (permissões, regra ativa sem vendedor → 400, edição sem deploy, validações — re-teste dia_semana=9 → 400 CONFIRMADO, prévia de horários). Perguntas ao suporte Skip registradas (modelo IA fast/reasoning, escolha de modelo/chave própria, limites por chamada, LGPD) — resposta decide IA 100% Skip vs serviço à parte.
- **REVISADO PELO BART — APROVADO** (provas de API e tela verdes; 2 notas baixas: campo desconhecido em janela gravou dia 0 — re-testar validação de dia_semana; PATCH/DELETE ia_envios id inexistente → 404, POST → 403 correto). Relatório: 06_notas/revisao-f0-ia-provas-bart.md
- Migration 0035: base_conhecimento (fonte obrigatória), regras_roteamento (Assinatura/Cíclico→Julio ativas; Pontual→Paulo inativa aguardando cadastro; Locação/Consultoria sem regra → fila do admin), janelas_executivo, ia_envios (só o sistema grava — nem admin), configuracoes.ia_modo=rascunho
- Hook ia_fundacao.js: permissões (CRUD das 3 coleções de cadastro só admin; base_conhecimento e ia_envios leitura só admin; ia_envios escrita SÓ pelo servidor — nem admin), validações (fonte obrigatória, dia_semana 0-6, início<fim, regra ativa exige vendedor, ia_modo ∈ {desligada,rascunho,automatica}, chave de configuração imutável), rota GET /backend/v1/ia/horarios-previa (só admin; enquanto a agenda Outlook não conecta → SEMPRE vazio c/ erro agenda_indisponivel — nenhum horário inventado)
- Tela /ia (só admin): modo de operação (3 botões, rascunho ativo), fila de envios (vazia, sem dado inventado), roteamento (5 modelos, Paulo aguardando cadastro), janelas de atendimento, base de conhecimento
- src/lib/horarios.ts + horarios.test.ts (6 testes; gêmea da lógica no hook — mudar nos dois)
- Aviso de rascunhos na Minha mesa

## 0.0.73 (14/11/2026) — Bloco D: importação do Pipedrive
- **APROVADO PELO BART** (provas a-f verdes na 1ª rodada); **SUSPENSO pelo owner** — histórico do Pipedrive não é confiável; NADA importado; rota e tela ficam prontas para reavaliação
- Migration 0034 (campos pipedrive_*), rota POST /backend/v1/pipedrive/importar (só admin; simular=true não grava; lotes de 500; dedup por pipedrive_id + empresa+pessoa), tela /importar-pipedrive
- Teste c/ planilha real: 3.541 ops + 4.753 leads + 2.612 fora; 0 EUR como valor; NADA gravado

## 0.0.72 (13/11/2026) — Bloco C: desempenho e relatórios
- **APROVADO PELO BART** (relatório 06_notas/revisao-bloco-c-provas-bart.md): rota /backend/v1/desempenho provada c/ login (admin 15/15 + vendedor 5/5 → 200; períodos mes/3m/6m/12m/todos); provas (a)-(f) verdes; D1 pré-0.0.72 corrigido (Outro sem observação → 400); A3 resolvido (sem frase inventada)
- Tela Desempenho c/ 6 destaques + 8 gráficos, Minha mesa na rota /, Responsável herdado do lead (só admin edita), 25 motivos de perda + "Outro (especificar)" inativo c/ observação obrigatória, meta 20/mês, moedas R$/US$ separadas, Tickets/Atendimentos fora do menu (rotas mantidas), erro dos logs corrigido
- Migration 0033 (desempenho)

## 0.0.69 (13/11/2026) — Bloco B: telas grandes
- **APROVADO PELO BART** (32/32 provas nos Blocos A+B): rota /oportunidades/:id em tela cheia (fim do modal 512px), bloqueio de etapa sem fechamento → 400 c/ mensagem clara, fontes ≥13px, "Manter-me conectado" funcional (cookie+flag)

## 0.0.68 (13/11/2026) — Bloco A/UX-A: segurança e estabilidade
- **APROVADO PELO BART** (24/24 provas; ressalva A3 resolvida no Bloco C): login sem credenciais expostas, "Solicitar acesso" sem contatos inventados, migration 0032 (qualquer usuário logado vê colegas — corrige regressão do Bloco 3), papel do usuário no header (fim do "Online" fixo), sem "v1.0.0" no rodapé

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **APROVADO PELO BART** (23/24 provas; D3 era falso positivo do script do Bart; D4 menor: cadastro de usuário por vendedor → 400 genérico em vez de 403)
- Migration 0031 (users.papel admin|vendedor), regras de users, hook permissoes_papel.js (exclusão só admin em 15 coleções; catálogo/configuração/demonstração só admin; último admin protegido; PROVA/DEMO palavra inteira), tela Equipe só admin

## 0.0.62–0.0.64 (13/11/2026) — Bloco 2: proteção, checklist, fechamento, taxas
- **APROVADO PELO BART** (provas de API e tela verdes): proteção contra exclusão (hook delete em 6 coleções; só PROVA/DEMO; real → 403 c/ sugestão de Arquivar; arquivamento reversível c/ motivo; exclusoes_auditoria), checklist por etapa (etapa_checklist_itens/respostas; imutáveis; insucesso → alerta c/ confirmação dupla), fechamento ganho/perda (motivo+concorrente obrigatórios; exige ambiente+valor>0; troca direta → 400; reabertura c/ justificativa; oportunidade_fechamentos só servidor), painel de taxas de passagem real × alvo 1.1a + win rate (meta 40%), cartão "Próximo passo"
- Migrations 0027-0030 (0030 = seed do checklist, conferida por API: 67 itens, sem duplicatas, idempotente; canônica 06_notas/conteudo-checklist-etapas-f2-t05.md)

## 0.0.60–0.0.61 (12/11/2026) — Bloco 1: entrada e pipeline
- **APROVADO PELO BART** (8 provas verdes): campos de entrada no lead (área de interesse/país/idioma/modelo de compra), fechamento (moeda/entidade derivadas no servidor; sem ambiente+valor → 400), 2 trilhas (13 etapas; RN-2.1; 1ª etapa automática), histórico de etapas (nada apagado), POST /backend/v1/leads/lote (1.000 leads em 0,4s — meta <1 min ATINGIDA), origem LP+Outras nas métricas
- Migrations 0024-0026

## 0.0.57–0.0.59 (12–13/11/2026) — Blocos 4 e 5 (Fase 1)
- Bloco 5 APROVADO (correções D1+D2: reconhecimento por nome+empresa; varredura determinística c/ retry e conferência de cobertura)
- Bloco 4 APROVADO (duplicidade contra a base inteira na prévia; telefone na importação; correção 0.0.57 plural pt-BR)
- Incidente 12/11: reimportação criou 7.548 duplicados — limpos c/ verificação por registro; base final 15.370

## 0.0.56 e anteriores (Fase 1)
- Fase 1 CONCLUÍDA 23/23 (13/11, teste do Bloco 5 aprovado pelo owner). Ver histórico no git.

## 2026-09-17

- Pasta operacional inicial criada para a Fase 1.
