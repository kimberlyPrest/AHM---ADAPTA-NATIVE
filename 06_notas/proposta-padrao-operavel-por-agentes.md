
Proposta — Padrão "Operável por humanos e agentes" (CRM + ERP + Chief of Staff)
Data: 24/11/2026 · Preparado por: Claude Code · Situação: RATIFICADO pelo owner em 24/11/2026 (os 10 requisitos, sem ajustes) — vira emenda 1.4a e critério de aceite de TODA task daqui para frente, no CRM e no ERP. Auditoria no código: aguarda o conector do Skip.

1. Visão do owner (24/11/2026)
Um Chief of Staff (CoS) — agente orquestrador — define e coordena os agentes de IA que trabalham no CRM (este projeto) e no ERP (outro projeto em construção no Claude Code).
O CoS também ajuda o owner a criar esses agentes.
Consequência: os sistemas passam a ter dois tipos de operador — humanos (que supervisionam, acompanham e decidem) e agentes (que executam). Tudo o que já foi construído precisa ser validado contra esse enquadramento, e tudo o que vier depois nasce dentro dele.
2. Divisão de responsabilidades
Camada	Responsável por	Não faz
Chief of Staff	Catálogo de agentes (quem existe, em qual sistema, qual missão, quem é o humano dono), despacho de tarefas, consolidação para o owner	Não guarda permissão: quem decide o que um agente pode fazer é o sistema onde ele atua
Sistema (CRM / ERP)	Identidade, permissão, regras de negócio, auditoria e fila de aprovação — tudo no servidor	Não confia no cliente (tela ou agente) para validar nada
Agente	Executa ações pela API com a própria identidade e dentro do nível de autonomia concedido	Não usa login de humano; não contorna a fila de aprovação
Humano	Supervisiona, aprova o que é decisão, ajusta autonomia, desliga agentes	—
Regra-mãe (já na emenda 1.3a, agora para todos os agentes): agente registra fatos; decisão ele sugere e um humano confirma — até o agente provar acerto e o owner subir a autonomia daquela ação.

3. Os 10 requisitos do padrão (critério de aceite de cada task)
#	Requisito	Pergunta de verificação na revisão
R1	Identidade própria — cada agente é um usuário do sistema (papel = agente), com credencial própria, dono humano e status ativo/pausado	A ação foi feita com o login do agente, nunca com o de um humano?
R2	Toda regra no servidor — nenhuma regra de negócio, estado derivado ou histórico depende da tela	Se a ação for feita só pela API (sem tela), o resultado é idêntico ao da tela?
R3	Permissão mínima por agente — o que cada agente lê/escreve é declarado e imposto no servidor	O agente leva 403 fora do seu escopo?
R4	Autonomia graduada por ação — desligada → rascunho (humano aprova) → automática, configurável por agente e tipo de ação	Existe fila de aprovação para esta ação, e o modo é editável sem deploy?
R5	Auditoria uniforme — todo registro grava quem fez (ator, tipo_ator humano/agente), origem (tela/API/agente X/integração), motivo e, quando agente, a fonte (e-mail, transcrição, trecho)	Dá para responder "quem fez, por quê e com base em quê" para qualquer alteração?
R6	Data do fato × data do registro — eventos têm ocorrido_em (editável, com auditoria) separado de created	Um fato de 3 dias atrás pode ser registrado com a data real?
R7	Idempotência — repetir a mesma chamada não duplica (chave de idempotência ou deduplicação natural)	Chamada repetida 2× gera 1 registro?
R8	Erros legíveis por máquina e por gente — resposta com codigo estável (para o agente) + mensagem em pt-BR corporativo (para o humano; regra de linguagem mantida)	O agente consegue decidir o que fazer só pelo código do erro?
R9	Eventos — mudanças relevantes (lead criado, etapa mudou, ganho/perda, envio aprovado) publicadas para agentes reagirem sem varrer a base	Um agente consegue saber o que mudou desde a última vez sem ler tudo?
R10	Supervisão humana visível — painel com o que cada agente fez, o que espera aprovação, erros e um botão de pausa por agente	O owner consegue pausar um agente agora e ver o que ele fez hoje?
4. Avaliação preliminar do que já existe (pela documentação — CONFIRMAR NO CÓDIGO)
Fonte: changelog 0.0.60–0.0.74, CLAUDE.md, briefings. A confirmação exige ler o código no Skip (conector precisa de autorização).

Req.	Situação	Evidência / lacuna
R1 Identidade	🔴 Falta	users.papel só tem `admin
R2 Regra no servidor	🟡 Parcial	✅ fechamento, proteção contra exclusão, checklist, permissões e roteamento são hooks. ❌ histórico json montado no frontend (regra do CLAUDE.md) — agente via API não grava histórico. ❌ Status do lead não acompanha a oportunidade (bug B1/B2 da Rose) — estado derivado precisa ir para o servidor. ⚠️ horários: lógica "gêmea" na tela e no hook. ⚠️ qualificação: confirmar onde roda
R3 Permissão mínima	🟡 Parcial	Permissão por papel existe (Bloco 3, 15 coleções), mas só 2 papéis amplos
R4 Autonomia	🟡 Semente	configuracoes.ia_modo (desligada/rascunho/automática) + ia_envios — é global e só para e-mail. Generalizar por agente e por ação
R5 Auditoria	🟡 Parcial	Existe por pedaço: exclusoes_auditoria, oportunidade_fechamentos, histórico de etapas, ia_envios. Falta campo uniforme tipo_ator/origem/fonte em todas as coleções que agentes vão escrever
R6 Data do fato	🔴 Falta	Datas das fases não editáveis (bug B4 da Rose)
R7 Idempotência	🟡 Parcial	Dedup na importação e no lote de leads; prevista no webhook da F1 (lead+gatilho). Não é regra geral
R8 Erros	🟡 Parcial	Mensagens pt-BR boas; códigos estáveis só em alguns casos (agenda_indisponivel)
R9 Eventos	🔴 Falta	Nenhum feed/webhook de saída documentado
R10 Supervisão	🟡 Semente	Tela /ia com modo e fila de envios. Falta visão por agente e pausa individual
Leitura: a base é boa (regras críticas já estão no servidor, há auditoria e modo rascunho). As lacunas são de padronização, não de reconstrução.

5. Como o feedback da Rose (24/11) se encaixa
Achado	Requisito que resolve
B1/B2 — lead "pendente" e painel "para trabalhar" após o ganho	R2 (estado derivado no servidor)
B4 — datas das fases não editáveis	R6 (ocorrido_em com auditoria)
P1 — observação livre por fase	R5 (fonte/observação) — e é o insumo que o agente de captura estrutura
P2 — anexo da proposta	R5 (evidência)
P3 — tipo de compra / recompra	Gatilho natural do primeiro agente de pós-venda (R4 em rascunho)
P4 — ocorrência na implementação	Coleção nova já nascida no padrão (R1–R10)
Ou seja: corrigir o feedback da Rose dentro do padrão resolve as duas coisas de uma vez.

6. Ordem proposta (cada item passa pelo portão do owner)
Ratificar este padrão (owner) → vira emenda 1.4a; o Bart passa a checar R1–R10 em toda revisão, além das provas e da linguagem.
Auditoria no código (Claude Code, só leitura no Skip) → confirma/corrige a tabela da seção 4, item por item.
Bloco R1 — fundação do padrão + bugs da Rose: papel agente, campos de auditoria uniformes, estado do lead derivado no servidor (B1/B2), ocorrido_em editável (B4), histórico gravado no servidor.
Bloco R2 — ajustes de processo da Rose (P1, P2, P4) já no padrão.
Bloco R3 — autonomia por agente + painel de supervisão (generaliza ia_modo/ia_envios; pausa por agente) e feed de eventos.
Só então os agentes da IA de vendas (F1 em diante) e o tipo de compra/recompra (P3) entram — já como agentes cadastrados.
7. CRM e ERP no mesmo padrão
O mesmo documento deve valer para o ERP, para que o Chief of Staff fale com os dois do mesmo jeito (mesma identidade de agente, mesmos campos de auditoria, mesmo formato de erro e de evento). Sugestão: manter a versão-mestre deste padrão num lugar comum aos dois projetos (ou no projeto do Chief of Staff) e cada sistema registrar a sua aderência.

Situação do ERP (24/11): em construção pelo owner no Claude Code do VS Code (outra plataforma, fora desta sessão). Avaliação de aderência: o Claude Code do ERP produz um relatório a partir do pedido em 06_notas/pedido-relatorio-erp.md; o owner traz o relatório para cá e a avaliação é feita contra R1–R10.

Situação do Chief of Staff (24/11): ainda não existe — é ideia do owner. Candidato natural: o Bart, com instruções reescritas (ver 06_notas/proposta-carta-bart-chief-of-staff.md).

8. Decisões do owner
✅ Ratifica os 10 requisitos (24/11/2026).
⏳ Auditoria no código (só leitura): depende de autorizar o conector do Skip.
⏳ Versão-mestre do padrão: sugestão — no projeto do Chief of Staff quando ele existir; até lá, neste arquivo.
⏳ Primeiro agente no padrão: sugestão — recompra/pós-venda (simples) antes do primeiro contato.
