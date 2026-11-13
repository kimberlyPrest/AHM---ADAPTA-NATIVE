# Estado atual — Adapta Cliente

- task_id: Bloco 3 da Fase 2 (F2-T11 permissões por papel) — implementado pelo CC (0.0.67, migration 0031), APROVADO pelo Bart
- champion: Rose
- spec: briefing 06_notas/briefing-f2-bloco3-permissoes.md
- etapa: aguardando_teste_humano — 23/24 provas verdes (relatório 06_notas/revisao-bloco3-provas-bart.md); o "D3" anterior era falso positivo do script do Bart (token como corpo em GET/DELETE → 400 genérico do PocketBase); nota D4 menor não bloqueante (cadastro de usuário por vendedor responde 400 genérico em vez de 403 corporativo — API rule falha antes do hook; padronização futura)
- teste_humano: Bloco 2 em teste com usuários (Julio e Rose, prazo 1 dia útil; roteiro 06_notas/roteiro-teste-f2-bloco2-usuarios.md) — plataforma liberada
- verificacao_automatica: passou (23/24; única divergência é a nota D4, comportamento funcional correto)
- aprendizado: em GET/DELETE o helper de prova nunca deve enviar corpo nem Content-Type (token como argumento posicional cai no body e vira JSON malformado → 400 genérico do PocketBase antes de regras e hooks)
- ultima_acao: bateria re-executada com script corrigido; limpeza das provas concluída (leads PROVA apagados, Demolidora arquivados, usuário de teste removido); base real intacta
- proxima_acao: fichas de Julio e Rose → Bart tabula e aplica critérios do protocolo; depois briefing do próximo bloco (IA — emenda 1.3a aguarda ratificação + 10 casos)
- atualizado_em: 2026-11-13T18:45:00-03:00
