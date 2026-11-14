# Changelog — CRM Atendimento AHM

## 0.0.72 (13/11/2026) — Bloco C: Minha mesa + Desempenho + Configurações
- **REVISADO PELO BART — APROVADO** (provas de API e tela verdes; D1 baixa: `exige_detalhe` do motivo "Outro" não cobrado no hook de fechamento — correção pequena). Relatório: 06_notas/revisao-bloco-c-provas-bart.md
- Minha mesa na rota `/` (saudação, resumo do mês, oportunidades c/ próximo passo, leads, transbordos; vendedor vê o dele, admin o time)
- Desempenho (`/desempenho`): 6 destaques + gráficos (criados×meta, ganhos/mês, abertas por etapa, funil, rosca ganhas×perdidas, perdas por motivo/etapa, tempo por etapa) + tabela por responsável (só admin); sem dados → "Sem dados no período", sem número inventado
- Configurações (só admin): meta mensal editável (seed 20), motivos de perda on/off
- Migration 0033: motivos_perda (25 do briefing + "Outro (especificar)" inativo c/ exige_detalhe), configuracoes, oportunidades.responsavel (backfill pelo lead), motivo_perda em oportunidades/fechamentos
- Hook desempenho.js: responsável herdado do lead; troca por não-admin → 403; rota agrega em SQL; vendedor forçado ao próprio id (prova: pediu outro id, servidor devolveu o dele)
- Perda exige motivo ativo da lista (prova: sem motivo → 400, inativo → 400, ativo → 200 + histórico); ganho mantém motivo em texto; reabrir limpa motivo
- Menu: Tickets e Atendimentos saíram (rotas vivas — prova: 200); sino de tickets removido
- Correção do erro dos logs: vendedor não pede mais histórico de fechamentos

## 0.0.69 (13/11/2026) — Bloco B: página da oportunidade + telas grandes
- **REVISADO PELO BART — APROVADO** (provas (a)-(e) e (g) verdes; (f) "Manter-me conectado" e (h) regressão completa ficam p/ próxima rodada). Relatório: 06_notas/revisao-bloco-b-provas-bart.md
- Nova rota `/oportunidades/:id` em tela cheia (antes modal de 512px): decisão no topo, "Próximo passo" em destaque, pipeline horizontal, checklist em 2 colunas, Fechamento/Resultado/Histórico na lateral
- Bloqueio guiado: sem ambiente/valor salvos, botões de etapa desabilitados c/ motivo ao lado e Fechamento em amarelo (resposta às 4 tentativas nos logs)
- Fila de oportunidades: linha abre a página; colunas Etapa e Valor; contadores filtram
- Escala de texto: nenhum texto de trabalho abaixo de 13px; tela inteira (limite de 1.280px removido); modais de lead/transbordo maiores
- "Manter-me conectado" funcionando

## 0.0.68 (13/11/2026) — Bloco UX-A: segurança e estabilidade
- **REVISADO PELO BART — APROVADO** (24/24 provas + regressão por papel na tela). Relatório: 06_notas/revisao-uxa-provas-bart.md. Ressalva: A3 (Dashboard c/ dado inventado) NÃO entrou — resolvido na 0.0.72 (Dashboard substituído pela Minha mesa)
- Login: quadro c/ credenciais de admin removido; campos abrem vazios; "Solicitar acesso" sem contatos inventados; título da aba "AHM Solution — Portal Comercial", lang pt-BR, noindex
- Migration 0032: qualquer usuário logado lista e vê os colegas (corrige regressão do Bloco 3 que as provas não pegaram)
- Layout: "Online" fixo → papel do usuário; "v1.0.0" removido do rodapé
- Pendência do owner: trocar a senha (exposta nas versões anteriores e no repositório)

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **REVISADO PELO BART — APROVADO** (23/24 provas; D3 era falso positivo do script de prova do Bart). Relatório: 06_notas/revisao-bloco3-provas-bart.md
- Migration 0031 users.papel; cadastro de usuários só admin (antes público); marcador PROVA/DEMO palavra inteira; lead real não renomeia p/ PROVA/DEMO; Equipe real c/ papéis; auditoria/fechamentos leitura só admin; último admin protegido
- Nota D4 menor: cadastro por vendedor → 400 genérico (regra da coleção barra antes do hook) — padronização pequena quando o Bart pedir
