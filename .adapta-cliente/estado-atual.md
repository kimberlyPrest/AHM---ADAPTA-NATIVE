# Estado atual — Adapta Cliente

- task_id: Bloco C da Fase 2 (F2-T13) — implementado pelo CC (0.0.72, migration 0033) e APROVADO pelo Bart (relatório 06_notas/revisao-bloco-c-provas-bart.md)
- champion: Rose
- spec: briefing 06_notas/briefing-f2-bloco-c-desempenho.md — tela Desempenho (réplica dos relatórios do Pipedrive), Minha mesa como tela inicial (Tickets/Atendimentos fora do menu), campo Responsável herdado do lead, lista fixa de 25 motivos de perda + "Outro" (inativo), meta do time 20 negócios/mês (configurável), etapas de negociação definidas, moedas separadas R$/US$, histórico de fechamentos só p/ admin
- etapa: aguardando_teste_humano (Julio e Rose — Bloco 2 + Minha mesa + Desempenho); Bloco D (importação Pipedrive c/ dedup) na sequência
- teste_humano: Bloco 2 em teste com usuários (Julio e Rose, prazo 1 dia útil; roteiro 06_notas/roteiro-teste-f2-bloco2-usuarios.md) + retorno sobre a página nova da oportunidade (UX-B)
- verificacao_automatica: passou — rota desempenho 21 combinações verdes (admin 15 + vendedor 5 + 401), provas (a)-(f) verdes, D1 pré-0.0.72 corrigido, A3 resolvido (sem dado inventado), logs limpos
- aprendizado: conferir o estado atual do Skip (versão + working tree) ANTES de escrever briefing/relatório; sondar formatos de parâmetros da rota antes da bateria (períodos mes/3m/6m/12m/todos ≠ 7/30/90)
- ultima_acao: revisão do Bloco C publicada (GitHub ee0c33a); "Outro (especificar)" restaurado inativo após as provas
- proxima_acao: fichas de Julio e Rose (Bloco 2 + Minha mesa + Desempenho) → tabulação; owner confirma os 25 motivos e decide ativar "Outro"; Bloco D (Pipedrive) briefing na sequência; emenda 1.3a (IA) depois
- atualizado_em: 2026-11-14T02:30:00-03:00
