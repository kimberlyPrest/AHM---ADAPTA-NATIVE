# Estado atual — Adapta Cliente

- task_id: IA de vendas F0+F1 (F2-T14/T15) — briefing 06_notas/briefing-f0-f1-ia-captura.md; F0 AUTORIZADA (F0.1-F0.5 CC pode começar; F0.6 = app Entra ID c/ TI da AHM), F1 aguarda APIs/especialistas/vídeos/Paulo/execução do app
- champion: Rose
- spec: briefing 06_notas/briefing-f0-f1-ia-captura.md — F0: base_conhecimento (fonte obrigatória), regras_roteamento (cadastro editável), janelas_executivo (∩ agenda real Outlook — nunca horário fixo sem conferir agenda), ia_envios (auditoria), modo rascunho (inicial, aceito)→automatica; F0.6 = app Entra ID NOVO (Mail.Read, Mail.Send caixa do CEO, Calendars.ReadWrite, transcrições; Application Access Policy → grupo "Comercial – IA") c/ administrador de TI da AHM; F1: webhook RD c/ secret, vínculo por e-mail normalizado (ambíguo NÃO envia), geração c/ citações reais, sem dor clara → fila do executivo do roteamento (NÃO transbordo), envio Graph caixa do CEO, interação no CRM
- etapa: F0 AUTORIZADA (modo rascunho; janelas ∩ agenda Outlook; app Entra ID = entrega nova F0.6 c/ TI da AHM; sem dor clara → fila do executivo do roteamento)
- teste_humano: Bloco 2 em teste com usuários (Julio e Rose, prazo 1 dia útil; roteiro 06_notas/roteiro-teste-f2-bloco2-usuarios.md) + retorno sobre a página nova da oportunidade (UX-B)
- verificacao_automatica: pendente — provas do Bart c/ protocolo novo após implementação
- aprendizado: conferir o estado atual do Skip (versão + working tree) ANTES de escrever briefing/relatório; sondar formatos de parâmetros da rota antes da bateria
- ultima_acao: briefing F0+F1 atualizado c/ ajustes do owner e publicado (GitHub 01a088f)
- proxima_acao: CC implementa F0.1-F0.5 → provas do Bart; owner encaminha especificação F0.6 ao TI da AHM e resolve pendências F1 (APIs RD/Envya, especialistas, vídeos, Paulo)
- atualizado_em: 2026-11-14T07:55:00-03:00
