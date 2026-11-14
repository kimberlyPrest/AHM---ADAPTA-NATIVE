# Briefing — Fatias F0 (Preparação) e F1 (Entrada inteligente) — IA de vendas AHM

**Autor**: Bart · 14/11/2026 · Fonte: `06_notas/analise-ia-captura-atividades.md` (análise do CC) + decisões do owner registradas em 14/11
**Status**: AGUARDANDO AUTORIZAÇÃO DO OWNER — nada implementado antes do "ok" explícito
**Atualização 14/11 07:29**: incorporados os ajustes do CC (P7 janelas × agenda real do Outlook; P8 modo rascunho/automático configurável; P9 sem dor clara → fila do executivo) + nota de verificação sobre os pontos atribuídos a mim que não estavam no briefing.

## Decisões do owner já registradas (base deste briefing)

1. **Primeiro e-mail AUTOMÁTICO, em nome do CEO (Afonso)** — velocidade máxima escolhida; **modo inicial (rascunho × automático) é decisão pendente do owner** — recomendação do Bart e do CC: rascunho nas primeiras 1-2 semanas
2. **Roteamento por modelo de negócio**: Julio → cíclico e assinatura; Paulo → pontual; Afonso e Rose ficam na gestão/estratégia
3. **Bônus/margens**: chegam depois (pasta `05_dados`, fora do GitHub); viram tabela editável pelo admin, nada fixo no código
4. **Pesos de impacto**: começam iguais (valor + recorrência + dor extraída; bônus com peso 0), ajustáveis pela tela com registro de cada ajuste
5. **Bloco D (Pipedrive)**: importação SUSPENSA pelo owner (histórico não confiável); tela pronta e parada; nada importado

## F0 — Preparação (o que entrega)

1. **Conexão Microsoft 365 — ENTREGA NOVA da F0 (não existe hoje)**, com permissões EXATAS (lista para o admin de TI):
   - app registration no Entra ID
   - Mail.Read das caixas do time comercial (grupo "Comercial – IA": Afonso, Rose, Julio, Paulo)
   - **Mail.Send SÓ da caixa do CEO**
   - Calendars.ReadWrite na agenda dos executivos (oferecer horários, criar reunião no Teams)
   - ler as transcrições das reuniões (transcrição automática ligada no Teams para o grupo)
   - política de acesso restrita ao grupo "Comercial – IA"
2. **Conexão Envya** (API ou webhook — PENDÊNCIA CRÍTICA do owner com o fornecedor): a conversa completa é a matéria-prima do primeiro e-mail
3. **Conexão RD Station** (API/webhook se liberado; fallback = e-mail de notificação que já recebem)
4. **Comunicado ao time** sobre o que é lido e para quê — **APROVADO PELO OWNER ANTES DA 1ª LEITURA** (LGPD/transparência)
5. **Cadastro do Paulo** na tela Equipe (o owner mesmo faz; CC não cria contas nem lida com senhas)

## F1 — Entrada inteligente (o que entrega)

Fluxo: RD Station pontua o lead → IA lê a conversa do Envya → cria/vincula o lead com resumo, dor, objetivo, produto provável e urgência → roteia (Julio/Paulo) → **envia, em nome do CEO**, o e-mail de primeiro contato: prova de entendimento da dor (palavras certas) · 1-3 perguntas de qualificação escolhidas pela necessidade · 2-3 horários da agenda do executivo (P7) · vídeo/business case quando fizer sentido · apresentação do especialista técnico do produto · executivo em cópia.

**Proteções obrigatórias do e-mail (decididas pelo owner — cada uma vira prova):**
- P1: o e-mail **só usa fatos** da conversa do Envya e do catálogo — **nunca preço, prazo ou compromisso comercial**
- P2: **estrutura do e-mail aprovada pelo owner antes de ligar** (template congelado até ele liberar)
- P3: **todo envio registrado no lead, com o texto exato** (quem/quando/o quê — auditável)
- P4: **botão de pausar os envios** a qualquer momento (efeito imediato)
- P5: **revisão diária** do que foi enviado, nas primeiras semanas
- P6: **executivo em cópia** (Julio ou Paulo conforme roteamento) — a resposta do cliente chega numa conversa que ele já está
- P7: **horários oferecidos = janelas preferidas × agenda real do Outlook** (ajuste do CC, 14/11): o cadastro define as janelas preferidas de cada executivo e a IA oferece só os horários dessas janelas que estiverem **livres na agenda real** (livre/ocupado) naquele momento — nunca horário fixo sem conferir a agenda
- P8: **modo de operação configurável** (rascunho × automático): a construção é a mesma, muda uma chave nas configurações; o modo inicial é decisão do owner (recomendação do Bart e do CC: **rascunho** nas primeiras 1-2 semanas — IA escreve, owner aprova com 1 clique — e automático quando ele estiver confortável; se o owner preferir automático desde o 1º dia, as proteções P1-P7 se mantêm)
- P9: **conversa sem dor clara → fila do executivo do roteamento** (contato humano), não transbordo (que é a fila técnica de especialista) — ajuste do CC, 14/11

## Pré-mortem — os jeitos de quebrar em produção (cada um vira prova antes do "go")

1. **E-mail errado em nome do CEO** (conteúdo inventado, preço citado, dor errada) → dano reputacional direto no nome do owner. **Prova**: e-mail gerado para conversa de teste sem dor clara → a IA deve recusar/ir para a fila do executivo (P9), não inventar; varredura do template por preço/prazo/compromisso → zero ocorrências; texto registrado no lead idêntico ao enviado.
2. **Vínculo errado de conversa → lead** (e-mail citando a dor de outro cliente) → **Prova**: 2 conversas de empresas diferentes no mesmo lote → cada e-mail cita a dor da conversa certa; lead duplicado (RD já cria?) → 1 registro só, com dedup por e-mail/empresa normalizados (mesma regra do Bloco D).
3. **Dependência da API do Envya** → sem a conversa, a IA não tem matéria-prima. **Mitigação**: F1 só entra no ar APÓS a API confirmada com conversa real de teste; fallback documentado (e-mail de notificação do RD) se o Envya não entregar.
4. **Escopo M365 maior que o prometido** (acesso a caixas fora do grupo) → **Prova**: conexão criada com as permissões da lista → tentativa de ler caixa fora do grupo → negada; log da conexão mostra só o grupo "Comercial – IA".
5. **E-mail interno/pessoal gravado** → **Prova**: e-mail interno entre Julio e Rose → zero registros criados; descarte sem gravação provado no log.
6. **Pausa que não pausa** (botão cosmético) → **Prova**: pausar → novo lead pontuado → NENHUM e-mail sai; retomar → fluxo volta.
7. **Roteamento opaco ou errado** (lead pontual para Julio) → **Prova**: lead cíclico → Julio; lead pontual → Paulo; a tela mostra POR QUÊ (regra determinística, não caixa-preta).
8. **Horário impossível oferecido ao cliente** (janela cai em reunião existente) → **Prova**: agenda com reunião marcada dentro da janela preferida → a IA NÃO oferece aquele horário (P7).

## Critério de acerto do piloto (definido ANTES de rodar)

- Amostra: conversas reais dos primeiros dias do piloto
- Medição: (a) dor resumida corretamente vs. a conversa; (b) produto provável correto; (c) roteamento correto; (d) e-mail sem fato inventado (conferência trecho de origem × texto enviado)
- Meta de autonomia: a IA só ganha autonomia (menos revisão) onde provar acerto — números publicados no repo ao fim de cada semana do piloto

## Pendências do owner (bloqueiam partes, não o todo)

| Pendência | Bloqueia |
|---|---|
| API/webhook do **Envya** (contato técnico do fornecedor) | **F1 inteira** (matéria-prima) |
| Acesso à API do **RD Station** | gatilho da F1 (fallback: e-mail de notificação) |
| **Nomes dos especialistas** por produto | parágrafo do especialista no e-mail |
| **Vídeos/business cases** por produto (links) | anexo do e-mail (pode lançar sem, se o owner preferir) |
| **Cadastro do Paulo** | roteamento pontual (fallback: tudo para Julio até lá) |
| **Admin do Microsoft 365** + comunicado ao time aprovado | **F0 inteira** |
| **Modo inicial** (rascunho × automático) | chave da F1b (decisão de 1 linha) |

**A F0 não depende de nenhuma pendência do owner além do admin de TI + comunicado** (confirmação do CC, 14/11): ela cria a base de conhecimento com fonte obrigatória, as regras de roteamento (editáveis pelo owner), as janelas de horário, o registro de todos os envios e a chave do modo de operação.

## Ordem de implementação proposta

**F0a** (comunicado aprovado + conexão M365 com as permissões + prova de escopo) → **F0b** (Envya/RD confirmados c/ conversa real de teste) → **F1a** (leitura da conversa → lead c/ resumo/dor/produto; SEM envio) → **F1b** (e-mail com as 9 proteções, no modo escolhido pelo owner, revisão diária ativa) → piloto medido.

Nada da F1b entra no ar antes de: comunicado aprovado, estrutura do e-mail aprovada, Envya entregando conversa real, e provas do pré-mortem verdes.

## Riscos além do escopo que registro desde já

> **Nota de verificação (Bart, 14/11 07:29)**: o CC apontou 3 "ajustes" ao briefing — (3) "não existe esteira do relatório diário" e o ajuste do transbordo. Verifiquei o texto publicado: **o briefing nunca mencionou "esteira do relatório diário" nem mandava lead sem dor clara para transbordo** (0 ocorrências de ambos; a conexão M365 já era entrega da F0). Provável confusão com rascunhos de conversa anterior. Os ajustes que MELHORAM o briefing foram incorporados como P7 (janelas × agenda real), P8 (modo rascunho/automático configurável) e P9 (fila do executivo). O modo inicial (rascunho × automático) é decisão do owner.

- **LGPD**: o comunicado deve dizer o que é lido, para quê, por quanto tempo fica guardado e como o vendedor questiona — minuta pronta para o owner antes da F0a
- **Retenção**: decisão do owner (decisão 4 da análise) ainda em aberto — proposta: transcrições completas guardadas 12 meses, campos extraídos sem prazo definido ainda; **fechar antes da F0a**
- **Custo de IA**: teto mensal definido antes do piloto (decisão da análise) — **fechar antes da F1b**
- **Marca do CEO**: se o e-mail sai da caixa do Afonso, toda resposta do cliente chega a ele — definir se a caixa tem delegação para Julio/Paulo responderem direto
