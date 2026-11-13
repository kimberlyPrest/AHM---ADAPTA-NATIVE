# Estado atual — Adapta Cliente

- task_id: Bloco UX-A da Fase 2 (F2-T12 segurança na tela) — briefing pronto (06_notas/briefing-f2-uxa-seguranca.md), autorização pendente do owner; Bloco 3 APROVADO pelo Bart (23/24)
- champion: Rose
- spec: briefing 06_notas/briefing-f2-uxa-seguranca.md — A1 login sem credenciais pré-preenchidas (bloqueante); A2 view/endpoint de nomes p/ vendedor (transbordo sem "Sem especialista atribuído" falso); A3 Dashboard sem frase inventada + SLA meta a decidir (configurável ou sai); A5 e.next() nos hooks after-success (achado do Bart nos logs); A6 verificação positiva das regras de auditoria
- etapa: aguardando_autorizacao
- teste_humano: Bloco 2 em teste com usuários (Julio e Rose, prazo 1 dia útil; roteiro 06_notas/roteiro-teste-f2-bloco2-usuarios.md)
- verificacao_automatica: pendente — provas do Bart c/ protocolo novo (pré-mortem + regressão por papel em TODAS as telas + varredura de segurança + logs)
- aprendizado: protocolo de revisão obrigatório (feedback do owner 13/11): pré-mortem, regressão por papel, varredura de segurança, dado inventado = defeito, logs, riscos fora do escopo, evidência antes de bloquear, seção "Riscos que encontrei além do pedido"
- ultima_acao: briefing do UX-A publicado c/ 2 achados do Bart além do que o CC já havia sinalizado (hooks sem e.next() nos logs; regras de auditoria verificadas)
- proxima_acao: owner autoriza UX-A → CC implementa → provas do Bart c/ protocolo novo. Em paralelo: fichas de Julio e Rose (Bloco 2)
- atualizado_em: 2026-11-13T17:20:00-03:00
