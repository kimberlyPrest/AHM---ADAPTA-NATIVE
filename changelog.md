# Changelog — CRM Atendimento AHM

## 0.0.69 (13/11/2026) — Bloco B: página da oportunidade + telas grandes
- **REVISADO PELO BART — APROVADO** (provas (a)-(e) e (g) verdes; (f) "Manter-me conectado" e (h) regressão completa ficam p/ próxima rodada). Relatório: 06_notas/revisao-bloco-b-provas-bart.md
- Nova rota `/oportunidades/:id` em tela cheia (antes modal de 512px): decisão no topo, "Próximo passo" em destaque, pipeline horizontal, checklist em 2 colunas, Fechamento/Resultado/Histórico na lateral
- Bloqueio guiado: sem ambiente/valor salvos, botões de etapa desabilitados c/ motivo ao lado e Fechamento em amarelo (resposta às 4 tentativas nos logs)
- Fila de oportunidades: linha abre a página; colunas Etapa e Valor; contadores filtram
- Escala de texto: nenhum texto de trabalho abaixo de 13px; tela inteira (limite de 1.280px removido); modais de lead/transbordo maiores
- "Manter-me conectado" funcionando

## 0.0.68 (13/11/2026) — Bloco UX-A: segurança e estabilidade
- **REVISADO PELO BART — APROVADO** (24/24 provas + regressão por papel na tela). Relatório: 06_notas/revisao-uxa-provas-bart.md. Ressalva: A3 (Dashboard c/ dado inventado) NÃO entrou — cobrar na próxima rodada
- Login: quadro c/ credenciais de admin removido; campos abrem vazios; "Solicitar acesso" sem contatos inventados; título da aba "AHM Solution — Portal Comercial", lang pt-BR, noindex
- Migration 0032: qualquer usuário logado lista e vê os colegas (corrige regressão do Bloco 3 que as provas não pegaram: transbordo "Sem especialista atribuído" falso, lista de especialistas só c/ o próprio vendedor, históricos sem "por quem")
- Layout: "Online" fixo → papel do usuário; "v1.0.0" removido do rodapé
- Pendência do owner: trocar a senha (exposta nas versões anteriores e no repositório)

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **REVISADO PELO BART — APROVADO** (23/24 provas; D3 era falso positivo do script de prova do Bart). Relatório: 06_notas/revisao-bloco3-provas-bart.md
- Migration 0031 users.papel; cadastro de usuários só admin (antes público); marcador PROVA/DEMO palavra inteira; lead real não renomeia p/ PROVA/DEMO; Equipe real c/ papéis; auditoria/fechamentos leitura só admin; último admin protegido
- Nota D4 menor: cadastro por vendedor → 400 genérico (regra da coleção barra antes do hook) — padronização pequena quando o Bart pedir

## 0.0.66 (13/11/2026) — Bloco UX-A (parcial)
- (registrado pelo CC; revisão consolidada na 0.0.68)
