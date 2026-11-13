# Estado atual — Adapta Cliente

- task_id: Bloco 3 da Fase 2 (F2-T11 permissões por papel) — implementado pelo CC (0.0.67, migration 0031), provas do Bart executadas
- champion: Rose
- spec: briefing 06_notas/briefing-f2-bloco3-permissoes.md
- etapa: em_correcao — D3 BLOQUEANTE encontrado nas provas (relatório 06_notas/revisao-bloco3-provas-bart.md, GitHub 087f7c0)
- teste_humano: Bloco 2 em teste com usuários (Julio e Rose, prazo 1 dia útil; roteiro 06_notas/roteiro-teste-f2-bloco2-usuarios.md) — IMPACTADO pelo D3: a tela está inutilizável enquanto o defeito persistir
- verificacao_automatica: 11 provas verdes (todas as regras de papel corretas: 403/404/400 nos bloqueios, auditoria 'administrador', palavra inteira, último admin protegido, vendedor opera o dia a dia) + D3 bloqueante: GET/DELETE autenticados → 400 'Something went wrong' em TODAS as coleções (POST/PATCH OK — hooks de request estouram; suspeito e.hasSuperuserAuth() no JSVM)
- aprendizado: header Authorization sem Bearer em GET também produz 400 genérico no PocketBase — nas provas, usar Accept: application/json e conferir o padrão de falha por método antes de concluir defeito
- ultima_acao: relatório da revisão publicado (087f7c0); D3 confirmado persistente (esperas até 10 min não recuperaram)
- proxima_acao: CC corrige permissoes_papel.js (try/catch defensivo ou verificação segura de admin em todos os callbacks de request) e reimplanta; Bart re-roda a bateria completa; depois limpeza dos leads de prova (Demolidora arquivados) e retomada do teste com usuários
- atualizado_em: 2026-11-13T18:30:00-03:00
