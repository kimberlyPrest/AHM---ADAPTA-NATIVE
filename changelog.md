# Changelog — CRM Atendimento AHM

## 0.0.74 (14/11/2026) — F0.1–F0.5 da IA de vendas: fundação
- **REVISADO PELO BART — APROVADO** (provas de API e tela verdes; 2 notas baixas: campo desconhecido em janela gravou dia 0 — re-testar validação de dia_semana; PATCH/DELETE ia_envios id inexistente → 404, POST → 403 correto). Relatório: 06_notas/revisao-f0-ia-provas-bart.md
- Migration 0035: base_conhecimento (fonte obrigatória), regras_roteamento (Assinatura/Cíclico→Julio ativas; Pontual→Paulo inativa aguardando cadastro; Locação/Consultoria sem regra → fila do admin), janelas_executivo, ia_envios (só o sistema grava — nem admin), configuracoes.ia_modo=rascunho
- Hook ia_fundacao.js: escrita por não-admin → 403; ia_envios imutável via API (POST admin → 403 provado); validações (fonte vazia, regra sem vendedor, janela início≥fim, modo inválido, chave imutável)
- Rota GET /backend/v1/ia/horarios-previa (só admin): janelas ∩ agenda; sem agenda conectada → vazio c/ erro agenda_indisponivel (não oferece horário impossível)
- src/lib/horarios.ts c/ 6 testes (agenda indisponível, sem janelas, 3 horários em dias diferentes no fuso de Brasília, compromisso empurra, antecedência 24h)
- Tela /ia (só admin): modo de operação (Desligada/Rascunho/Automática), fila de envios, roteamento editável, janelas c/ prévia, base de conhecimento
- Aviso de rascunhos pendentes na Minha mesa (admin)
- Nenhum e-mail é enviado nesta fase; pendências p/ F0.6-F1: admin M365, API RD/Envya, especialistas, materiais, Paulo

## 0.0.73 (14/11/2026) — Bloco D: importação do histórico do Pipedrive
- **REVISADO PELO BART — APROVADO** (provas (a)-(f) verdes na 1ª rodada, planilha PROVA sintética). Relatório: 06_notas/revisao-bloco-d-provas-bart.md. **Importação real SUSPENSA pelo owner** (histórico não confiável — meio do caminho frágil); tela pronta e parada; nada importado; opção futura: só ganhos do Comercial conferidos c/ financeiro
- Migration 0034: campos pipedrive_* em oportunidades e leads; rota POST /backend/v1/pipedrive/importar (só admin, simular=true não grava, lotes de 500, dedup por pipedrive_id + empresa+pessoa); mapeamento no navegador (src/lib/pipedrive.ts c/ testes); tela /importar-pipedrive; planilha real em 05_dados/ (fora do GitHub)

## 0.0.72 (13/11/2026) — Bloco C: Minha mesa + Desempenho + Configurações
- **REVISADO PELO BART — APROVADO** (provas de API e tela verdes; D1 inicial era artefato do script do Bart — re-testado: exige_detalhe do "Outro" bloqueia corretamente). Relatório: 06_notas/revisao-bloco-c-provas-bart.md
- Minha mesa na rota `/`; Desempenho c/ 6 destaques + gráficos + tabela por responsável (só admin); Configurações (meta 20, motivos on/off); Responsável herdado do lead; perda exige motivo da lista; Tickets/Atendimentos fora do menu (rotas vivas); erro dos logs corrigido
- Decisões do owner: "Outro" fica DESATIVADO; lista fica c/ os 25 motivos

## 0.0.69 (13/11/2026) — Bloco B: página da oportunidade + telas grandes
- **REVISADO PELO BART — APROVADO** (provas (a)-(e) e (g) verdes; (f) e (h) p/ próxima rodada). Relatório: 06_notas/revisao-bloco-b-provas-bart.md
- Rota /oportunidades/:id em tela cheia; bloqueio guiado c/ motivo; fila c/ Etapa/Valor; escala ≥13px; Manter-me conectado funcionando

## 0.0.68 (13/11/2026) — Bloco UX-A: segurança e estabilidade
- **REVISADO PELO BART — APROVADO** (24/24 provas + regressão por papel na tela). Relatório: 06_notas/revisao-uxa-provas-bart.md. Ressalva A3 resolvida na 0.0.72
- Login sem credenciais expostas; migration 0032 (vendedor vê colegas); "Online"→papel; "v1.0.0" fora; pendência: senha do owner

## 0.0.67 (13/11/2026) — Bloco 3: permissões por papel
- **REVISADO PELO BART — APROVADO** (23/24 provas; D3 era falso positivo do script do Bart). Relatório: 06_notas/revisao-bloco3-provas-bart.md
- Migration 0031 users.papel; cadastro só admin; PROVA/DEMO palavra inteira; Equipe real; auditoria só admin; último admin protegido
