# Estado atual — Adapta Cliente

- task_id: F1-T09
- champion: Rose
- spec: 04_fase-atual/specs/spec-1.3-motor-qualificacao-score.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada 2026-11-10 20:26 BRT — "pode implementar" (owner Afonso)
- teste_humano: pendente
- verificacao_automatica: passou — Skip 0.0.26 (hash 506641b), QA completo OK; provas: produto INATIVO → qualificação bloqueada (400 com nome do produto e instrução); produto INEXISTENTE → bloqueada (400); lead SEM produto → bloqueada (400, RN-1.6); lead preservado em todos os bloqueios (nome/empresa/produto intactos); caminho de recuperação (trocar para produto ativo → qualifica, HTTP 200); regressão CA-1-007 OK; base intacta (15.370 leads, 1 produto)
- aprendizado: pendente
- ultima_acao: F1-T09 implementada (hook on_leads_qualificar.js revalida produto: inexistente/inativo/ausente → 400 sem tocar no lead; modal Editar mostra produto atual mesmo inativo marcado "(inativo)")
- proxima_acao: aguardar teste humano (Afonso testa bloqueio e recuperação pela tela)
- atualizado_em: 2026-11-10T20:35:00-03:00
