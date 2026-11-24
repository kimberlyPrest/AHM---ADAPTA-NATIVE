
Análise — Feedback da Rose sobre o teste do CRM (24/11/2026)
Fonte: 2 e-mails da Rose (24/11, 09:48 e 10:40), em resposta ao convite de teste de 13/11 (roteiro roteiro-teste-f2-bloco2-usuarios.md). Situação: ANÁLISE — nada implementado. Bugs precisam ser reproduzidos por API/tela antes de virar task (Skip MCP sem autenticação nesta sessão).

Casos testados
Caso A (Red Zone): cliente entrou em contato, não quis reunião, recebeu proposta, sumiu, voltou dias depois e fechou. Fluxo: lead → oportunidade → perda (sem retorno) → reabertura → ganho.
Caso B (Veronica Ruiz, Mini Clips): compra com avaria sem acionamento do dispositivo (problema na implementação). Ganho registrado; etapa Implementação deixada aberta.
Os dois casos são reais e não-lineares — exatamente o que o roteiro com registros PROVA não cobria.

Achados classificados
1. Bugs — o sistema mostra informação falsa (prioridade máxima)
#	Achado	Hipótese	Ligação
B1	Oportunidade ganha, mas o lead continua pendente na tela de Leads ("parece que não foi trabalhado")	Status do lead não é atualizado pelo andamento/fechamento da oportunidade	Reincidência do AP-2026-11-11-0600 (efeito visível)
B2	No Dashboard a oportunidade ganha aparece como "para trabalhar"	Painel lê o status do lead, não o da oportunidade (mesma causa de B1?)	AP-2026-11-11-0600
B3	Trocar o produto na tela do lead "não alterou"	A confirmar: a oportunidade não herda a troca, ou a qualificação não é reprocessada	Reproduzir antes de concluir
B4	Datas das 5 fases do AHM Cycle não são editáveis	RN-2.2 grava a data de entrada automaticamente; casos retroativos ficam com datas erradas	Data editável com auditoria (data real × data do registro)
2. Cadastro / insumo (não é código)
C1 — Catálogo só tem ShockWatch. Red Zone (Red Light) e os demais produtos das emendas 1.1a/1.2b (8 de baixa + 12 de alta complexidade) não estão cadastrados. Como a qualificação exige produto, todo lead fora do ShockWatch fica "pendente" ou é forçado a um produto errado (foi o que a Rose fez no teste). Mini Clips também não aparece nas emendas — confirmar família.
3. Ajustes de processo — o funil está mais rígido que a venda real
#	Pedido	Leitura
P1	Campo de observações livre em cada fase	Perguntas fixas não cabem em casos como reposição de faróis
P2	Anexar PDF da proposta na fase Proposta Customizada	Campo arquivo na oportunidade/fase
P3	Tipo de compra: compra inicial · piloto · recompra/reposição	Recompra não exige ROI nem piloto → checklist e fases diferentes por tipo. Conversa com a Fase 5 (recompra PUSH 45–120 dias)
P4	Implementação: insucessos limitados; sem como registrar o problema ocorrido e as ações	Precisa de "ocorrência" (problema, causa, ação, responsável, prazo, status) — caso Veronica fica aberto até isso existir
Ligação com a IA de vendas (o que o feedback valida)
O registro manual é o gargalo: a Rose precisou lançar à mão cada interação de um caso simples. É o mesmo diagnóstico que suspendeu o Pipedrive (Bloco D) e justifica a captura automática de e-mails/agenda/transcrições (analise-ia-captura-atividades.md).
Vendas reais não seguem o funil linear (sem reunião, sumiço, retorno, reposição): a IA deve inferir etapa e tipo de compra pelo conteúdo, e o funil precisa aceitar pular/voltar etapas com justificativa.
Observação livre (P1) é o insumo que a IA estrutura depois (dor, aplicação, objeções) — vale exigir texto livre e deixar a IA extrair, em vez de multiplicar perguntas fixas.
Recompra (P3) é o caso mais simples de automatizar: lembrete PUSH 45–120 dias após o ganho, com e-mail em rascunho.
Itens de atenção
O e-mail de convite (13/11) enviou login e senha em texto para 3 pessoas (com cópia). Trocar as senhas de teste do Julio e da Rose.
Os dois casos foram lançados com dados reais (sem prefixo PROVA). Decidir se ficam como registros reais ou são marcados/arquivados.
O Julio não respondeu (até onde consta).
Proposta de ordem (sujeita ao portão do owner)
Bloco R1 — bugs B1-B4 (sincronizar lead/painel com a oportunidade; data editável com auditoria) + reproduzir B3.
Insumo C1 — owner/Rose entregam a lista de produtos; cadastro via API.
Bloco R2 — P1, P2, P4 (observação livre, anexo da proposta, ocorrência na implementação).
Bloco R3 — P3 (tipo de compra com trilha própria para recompra) — desenhar junto com a IA (PUSH de recompra).
