# Changelog — CRM Atendimento AHM

## 0.0.73 (14/11/2026) — Bloco D: importação do histórico do Pipedrive
- **REVISADO PELO BART — APROVADO** (provas (a)-(f) verdes na 1ª rodada, planilha PROVA sintética). Relatório: 06_notas/revisao-bloco-d-provas-bart.md. Importação real liberada p/ o owner após conferência da amostra na prévia
- Migration 0034: oportunidades.pipedrive_id (índice único)/pipedrive_proprietario/pipedrive_produto/valor_original/moeda_original; leads.pipedrive_id (único)/pipedrive_criado_em/pipedrive_etapa
- Rota POST /backend/v1/pipedrive/importar (só admin, 403 provado): simular=true não grava nada; gravação em lotes de 500 c/ transação; dedup por pipedrive_id + empresa+pessoa normalizados (mesmo mapa de acentos do front); reimportação idempotente (prova: 2ª rodada = 0 novos)
- Mapeamento no navegador (src/lib/pipedrive.ts c/ testes): funis Comercial/Pós-venda/LATAM → oportunidades trilha alta (Fechamento→Negociação, decisão 1); Pré-vendas → leads (decisão 3); Conversa Abandonada/Testes fora (decisão 4); motivos: 25 + sinônimos aprovados, resto sem classificação c/ texto guardado (decisão 5); EUR/outras → só valor original guardado, nunca convertido (decisão 6); tudo desde 2011 (decisão 7)
- Leads novos: rascunho/pendente, sem contato inventado, etapa PD registrada, observação pedindo completar contato
- Exceções só p/ importados (pipedrive_id preenchido): sem ambiente mantém moeda original; sem produto mantém trilha; não cria registro de passagem; desempenho não herda responsável do lead
- Desempenho: criadas/duração/funil usam datas originais do Pipedrive (prova: porMes mostra 2023-06/2024-02/2024-11/2025-03)
- Tela /importar-pipedrive (só admin, link em Desempenho): prévia c/ contagens por etapa/status, amostra de 25 ligações a leads existentes, responsáveis (verde = mapeado), falhas; botão Importar só após prévia, c/ confirmação; conferência final vs. prévia
- Planilha real em 05_dados/ (fora do GitHub, .gitignore) — dados de clientes nunca vão ao repositório

## 0.0.72 (13/11/2026) — Bloco C: Minha mesa + Desempenho + Configurações
- **REVISADO PELO BART — APROVADO** (provas de API e tela verdes; D1 inicial era artefato do script do Bart — re-testado: exige_detalhe do "Outro" bloqueia corretamente). Relatório: 06_notas/revisao-bloco-c-provas-bart.md
- Minha mesa na rota `/` (saudação, resumo do mês, oportunidades c/ próximo passo, leads, transbordos; vendedor vê o dele, admin o time)
- Desempenho (`/desempenho`): 6 destaques + gráficos + tabela por responsável (só admin); sem dados → "Sem dados no período", sem número inventado
- Configurações (só admin): meta mensal editável (seed 20), motivos de perda on/off
- Migration 0033: motivos_perda (25 do briefing + "Outro (especificar)" inativo c/ exige_detalhe), configuracoes, oportunidades.responsavel (backfill pelo lead), motivo_perda em oportunidades/fechamentos
- Hook desempenho.js: responsável herdado do lead; troca por não-admin → 403; rota agrega em SQL; vendedor forçado ao próprio id
- Perda exige motivo ativo da lista; ganho mantém motivo em texto; reabrir limpa motivo
- Menu: Tickets e Atendimentos saíram (rotas vivas); sino de tickets removido; erro dos logs corrigido (vendedor não pede mais histórico de fechamentos)
- Decisões do owner: "Outro" fica DESATIVADO; lista fica c/ os 25 motivos

## 0.0.69 (13/11/2026) — Bloco B: página da oportunidade + telas grandes
- **REVISADO PELO BART — APROVADO** (provas (a)-(e) e (g) verdes; (f) "Manter-me conectado" e (h) regressão completa ficam p/ próxima rodada). Relatório: 06_notas/revisao-bloco-b-provas-bart.md
- Nova rota `/oportunidades/:id` em tela cheia (antes modal de 512px): decisão no topo, "Próximo passo" em destaque, pipeline horizontal, checklist em 2 colunas, Fechamento/Resultado/Histórico na lateral
- Bloqueio guiado: sem ambiente/valor salvos, botões de etapa desabilitados c/ motivo ao lado e Fechamento em amarelo
- Fila de oportunidades: linha abre a página; colunas Etapa e Valor; contadores filtram
- Escala de texto: nenhum texto de trabalho abaixo de 13px; tela inteira; modais maiores; "Manter-me conectado" funcionando

## 0.0.68 (13/11/2026) — Bloco UX-A: segurança e estabilidade
- **REVISADO PELO BART — APROVADO** (24/24 provas + regressão por papel na tela). Relatório: 06_notas/revisao-uxa-provas-bart.md. Ressalva A3 resolvida na 0.0.72 (Dashboard substituído pela Minha mesa)
- Login: quadro c/ credenciais de admin removido; campos abrem vazios; "Solicitar acesso" sem contatos inventados; título da aba novo, lang pt-BR, noindex
- Migration 0032: qualquer usuário logado lista e vê os colegas (corrige regressão do Bloco 3)
- Layout: "Online" fixo → papel do usuário; "v1.0.0" removido
- Pendência do owner: trocar a senha (exposta nas versões anteriores e no repositório)

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **REVISADO PELO BART — APROVADO** (23/24 provas; D3 era falso positivo do script de prova do Bart). Relatório: 06_notas/revisao-bloco3-provas-bart.md
- Migration 0031 users.papel; cadastro de usuários só admin; marcador PROVA/DEMO palavra inteira; lead real não renomeia p/ PROVA/DEMO; Equipe real c/ papéis; auditoria/fechamentos leitura só admin; último admin protegido
- Nota D4 menor: cadastro por vendedor → 400 genérico (regra da coleção barra antes do hook) — padronização pequena quando o Bart pedir
