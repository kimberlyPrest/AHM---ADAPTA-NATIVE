# Briefing — IA de vendas: F0 (fundação) + F1 (primeiro contato automático) — F2-T14/T15

**Data**: 14/11/2026 · **F0 AUTORIZADA pelo owner (14/11), com os ajustes abaixo incorporados** · F1 aguarda pendências · **Base**: emenda 1.3a (IA nativa Skip, sem credencial de terceiros no caminho crítico) + decisões do owner.

## O que o owner aprovou

Quando o RD Station pontua o lead, a IA lê a conversa do Envya e envia, **automaticamente e em nome do CEO**, um e-mail com:
1. Prova de entendimento da dor (extraída da conversa real)
2. 1 a 3 perguntas de qualificação
3. 2 ou 3 horários do executivo — **janelas preferidas cruzadas com a agenda real do Outlook (livre/ocupado)**, nunca horário fixo sem conferir a agenda
4. Vídeo ou business case, quando fizer sentido
5. O especialista técnico do produto (nomeado no e-mail)

**Roteamento**: Julio → recorrente e assinatura · Paulo → pontual.

**Ajustes do owner na autorização da F0 (14/11)**:
- **Modo inicial**: `rascunho` (aceito pelo owner)
- **`horarios_executivo` = janelas preferidas** — os horários oferecidos são essas janelas cruzadas com a agenda real do Outlook (livre/ocupado); **nunca horário fixo sem conferir a agenda**
- **Não existe "esteira do relatório diário" nem integração com o Microsoft 365 herdada de outro fluxo** — o registro do app no Entra ID e as permissões (**Mail.Read, Mail.Send só na caixa do CEO, Calendars.ReadWrite, transcrições**) e a política de acesso restrita ao grupo **"Comercial – IA"** entram na **F0 como entrega nova**, com o administrador de TI da AHM
- **Conversa sem dor clara → fila do executivo do roteamento** (NÃO transbordo — transbordo é a fila técnica de especialista)

## Pendências (bloqueiam partes — o desenho segue sem elas)

1. Acesso à API do RD Station (webhook de pontuação) e do Envya (leitura da conversa) — e-mail de solicitação pronto, owner envia ao especialista
2. Nomes dos especialistas técnicos por produto/família
3. Vídeos e business cases por produto
4. Cadastro do Paulo (usuário vendedor) — Bart cria via API assim que o owner passar nome + e-mail
5. **Execução do app no Entra ID pelo administrador de TI da AHM** (entrega da F0 — ver F0.6); credenciais por canal seguro

## F0 — Fundação (autorizada; começa já)

**Princípio da emenda 1.3a**: nenhuma dependência de credencial de terceiros no caminho crítico; IA nativa Skip; regras em cadastro editável pelo owner; toda recomendação cita a fonte; sem fonte → transbordo.

### F0.1 — Base de conhecimento consultável
- Coleção `base_conhecimento` (tipo: produto | familia | faq | caso | video | business_case | especialista; título, conteúdo, produto/família de referência, ativo)
- Toda entrada com `fonte` (URL ou documento) — a IA cita; sem fonte → não usa
- CRUD só admin; leitura pela IA (server-side)
- Seed inicial: catálogo de produtos (já existe), famílias de diagnóstico (emenda 1.1a), especialistas (quando o owner passar os nomes)

### F0.2 — Regras de roteamento em cadastro (não em código)
- Coleção `regras_roteamento` (modelo de compra → vendedor; ativo; prioridade)
- Seed: recorrente→Julio, assinatura→Julio, pontual→Paulo (quando cadastrado)
- Editável pelo owner na tela (sem deploy para mudar regra)

### F0.3 — Janelas preferidas do executivo (cadastro) + agenda real
- Coleção `janelas_executivo` (dia da semana, janela início–fim, ativo) — **janelas preferidas, editáveis pelo owner**
- Os horários oferecidos no e-mail são **janelas ∩ agenda real do Outlook (livre/ocupado)** via Graph `Calendars.Read`/`Calendars.ReadWrite` — nunca horário fixo sem conferir a agenda
- Se a agenda do executivo não estiver acessível (falha/sem credencial), **nenhum horário é oferecido** — o e-mail vira rascunho com alerta (nunca inventa horário)

### F0.4 — Auditoria de envio da IA
- Coleção `ia_envios` (lead, gatilho, rascunho do e-mail, versão enviada, status, quem autorizou se manual, criado_em) — escrita só pelo servidor
- Todo envio (auto ou manual) registrado; o owner audita

### F0.5 — Modo de operação configurável
- `configuracoes.ia_modo`: `desligada` | `rascunho` | `automatica` — **inicial: `rascunho` (aceito pelo owner)**
- **rascunho**: a IA gera o e-mail, grava em `ia_envios` e NOTIFICA o owner no app — nada sai sem clique humano
- **automatica**: envia direto (só depois de o owner validar rascunhos por algumas semanas)
- A tela de Minha mesa/Desempenho mostra a fila de rascunhos quando o modo é rascunho

### F0.6 — Identidade de envio no Entra ID (entrega NOVA da F0 — com o administrador de TI da AHM)
- **Não há "esteira do relatório diário" nem integração M365 herdada**: este app é próprio do fluxo de IA de vendas
- App registration no Entra ID (single tenant), permissões de aplicação: **Mail.Read, Mail.Send (restrita à caixa do CEO), Calendars.ReadWrite, transcrições de reunião** — com consentimento de administrador
- **Application Access Policy** no Exchange Online restrita ao grupo **"Comercial – IA"**
- Credenciais (Tenant ID, Client ID, Client Secret) entregues **por canal seguro**; guardadas em secret do projeto (nunca em repo/código)
- Responsável: administrador de TI da AHM; o Bart fornece a especificação exata e testa o primeiro envio/leitura de agenda no dia da entrega

## F1 — Primeiro contato automático (depende das pendências 1, 2, 3 e da execução da F0.6)

### F1.1 — Gatilho RD Station
- Webhook de entrada (endpoint público protegido por secret) recebe a pontuação do lead
- Validação: secret no header; payload inválido → 400 sem processar; log de todas as chamadas

### F1.2 — Vínculo conversa ↔ lead (o risco do vínculo errado)
- A conversa do Envya é vinculada ao lead por **e-mail normalizado** (mesma normalização da importação) + fallback por telefone
- **Regra de segurança**: se o vínculo for ambíguo (2+ leads com o mesmo e-mail), a IA NÃO envia — vira rascunho com alerta de conflito para o owner
- A conversa lida é guardada em `ia_envios.conversa_resumo` (trechos citados que provam a dor)

### F1.3 — Geração do e-mail (IA nativa Skip)
- Prompt com: resumo da conversa (trechos reais), produto/família, especialista do cadastro, janelas do cadastro, vídeo/business case do cadastro
- **Prova de entendimento da dor**: só com trechos da conversa real (citação)
- **Conversa sem dor clara → fila do executivo do roteamento** (o lead segue para o vendedor responsável pelo modelo de compra) — **NÃO transbordo**, que é a fila técnica de especialista
- **Perguntas de qualificação**: 1 a 3, vindas das regras por produto (cadastro F0.1)
- **Horários**: 2 ou 3 resultantes de **janelas preferidas ∩ agenda real (livre/ocupado)** — se a agenda não responde, sem horário no e-mail (rascunho com alerta)
- **Vídeo/business case**: só se o cadastro tiver para aquele produto e a dor casar
- **Especialista**: nome do cadastro F0.1 — nunca inventado
- Assinatura em nome do CEO (texto fixo aprovado pelo owner no cadastro)

### F1.4 — Envio (app da F0.6)
- Envio via Graph Mail.Send restrito à caixa do CEO (Application Access Policy → grupo "Comercial – IA")
- Falha de envio → status em `ia_envios` + alerta; **nunca reenviar automaticamente** (risco de duplicar contato com cliente)

### F1.5 — Registro no CRM
- O e-mail enviado vira interação no lead (tipo "e-mail IA", com link para `ia_envios`)
- Resposta do cliente entra como interação normal (o vendedor vê o histórico completo)

## Pré-mortem do Bart (5 jeitos de quebrar em produção — com prova planejada)

1. **E-mail em nome do CEO com dado errado** (dor inventada, especialista errado, horário impossível) → prova: modo rascunho obrigatório no início; cada rascunho conferido contra a conversa real; varredura de dado inventado (todo trecho de dor deve existir na conversa; todo horário deve existir na agenda livre)
2. **Vínculo errado de conversa** (e-mail do lead A com conversa do lead B) → prova: caso ambíguo → NÃO envia, vira rascunho com alerta; caso único → vínculo correto conferido
3. **Dependência do Envya derrubar o fluxo** (API fora/timeout/mudança de formato) → prova: falha simulada → o lead NÃO perde o registro (fica pendente com status de erro), alerta ao owner, retry manual — nunca silencioso
4. **Duplicação de contato** (RD reenvia o webhook; lead pontuado 2×) → prova: mesmo webhook 2× → 1 só e-mail (idempotência por lead+gatilho)
5. **Webhook público vira porta de entrada** (spam/forgery) → prova: sem secret → 401; payload inválido → 400; rate limit; nada grava sem secret válido

## Critérios de aceite (provas do Bart)

1. F0: cadastros CRUD só admin; IA lê server-side; regras de roteamento editáveis sem deploy; **nenhum horário oferecido sem conferência de agenda real** (agenda indisponível → sem horário + alerta)
2. Modo rascunho: nenhum e-mail sai sem clique humano; fila visível ao owner; auditoria completa em `ia_envios`
3. F1 (quando destravado): webhook com secret; vínculo ambíguo não envia; e-mail com citações reais da conversa; falha do Envya não perde lead; idempotência de gatilho; **sem dor clara → fila do executivo do roteamento (não transbordo)**
4. Regressão por papel em todas as telas + varredura de dado inventado nos e-mails gerados
5. Logs limpos após a rodada de provas

## Ordem de implementação

1. **F0.1–F0.5** (cadastros + auditoria + modo rascunho) — CC pode começar já
2. **F0.6** em paralelo: owner encaminha a especificação ao administrador de TI da AHM; Bart testa credenciais no dia da entrega
3. Owner resolve pendências (APIs, especialistas, vídeos, Paulo)
4. **F1.1–F1.2** (webhook + vínculo) em modo rascunho
5. Owner valida rascunhos por 1–2 semanas
6. **F1.3–F1.5** (geração + envio) — modo automatica só depois da validação

## Nota sobre o Bloco D

Importação do Pipedrive **suspensa pelo owner** (histórico não confiável) — nada foi importado; a rota e a tela ficam prontas para quando ele reavaliar. Sem impacto no F0/F1.
