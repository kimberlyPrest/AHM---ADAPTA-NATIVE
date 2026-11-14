# Briefing — IA de vendas: F0 (fundação) + F1 (primeiro contato automático) — F2-T14/T15

**Data**: 14/11/2026 · **Autorização**: owner aprovou a direção (análise do CC em 06_notas/analise-ia-captura-atividades.md — sessão do CC; consolidar no repo quando possível) · **Base**: emenda 1.3a (IA nativa Skip, sem credencial de terceiros no caminho crítico) + decisões do owner.

## O que o owner aprovou

Quando o RD Station pontua o lead, a IA lê a conversa do Envya e envia, **automaticamente e em nome do CEO**, um e-mail com:
1. Prova de entendimento da dor (extraída da conversa real)
2. 1 a 3 perguntas de qualificação
3. 2 ou 3 horários do executivo
4. Vídeo ou business case, quando fizer sentido
5. O especialista técnico do produto (nomeado no e-mail)

**Roteamento**: Julio → recorrente e assinatura · Paulo → pontual.

**Riscos óbvios declarados pelo CC** (o pré-mortem abaixo os formaliza): envio automático externo em nome do CEO · vínculo errado de conversa · dependência da API do Envya.

## Pendências do owner (bloqueiam partes — o desenho segue sem elas)

1. Acesso à API do RD Station (webhook de pontuação) e do Envya (leitura da conversa)
2. Nomes dos especialistas técnicos por produto/família
3. Vídeos e business cases por produto
4. Cadastro do Paulo (usuário vendedor)
5. Administrador do Microsoft 365 (para o envio em nome do CEO — mesma esteira do relatório diário: app registration + Mail.Send + Application Access Policy)

## F0 — Fundação (sem dependência externa, pode começar já)

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

### F0.3 — Cadastro de horários do executivo
- Coleção `horarios_executivo` (dia da semana, hora, ativo) — os "2 ou 3 horários" do e-mail saem daqui, nunca inventados
- Editável pelo owner

### F0.4 — Auditoria de envio da IA
- Coleção `ia_envios` (lead, gatilho, rascunho do e-mail, versão enviada, status, quem autorizou se manual, criado_em) — escrita só pelo servidor
- Todo envio (auto ou manual) registrado; o owner audita

### F0.5 — Modo de operação configurável
- `configuracoes.ia_modo`: `desligada` | `rascunho` | `automatica` (default **rascunho**)
- **rascunho**: a IA gera o e-mail, grava em `ia_envios` e NOTIFICA o owner no app — nada sai sem clique humano
- **automatica**: envia direto (só depois de o owner validar rascunhos por algumas semanas)
- A tela de Minha mesa/Desempenho mostra a fila de rascunhos quando o modo é rascunho

## F1 — Primeiro contato automático (depende das pendências 1, 2, 3, 5)

### F1.1 — Gatilho RD Station
- Webhook de entrada (endpoint público protegido por secret) recebe a pontuação do lead
- Validação: secret no header; payload inválido → 400 sem processar; log de todas as chamadas

### F1.2 — Vínculo conversa ↔ lead (o risco do vínculo errado)
- A conversa do Envya é vinculada ao lead por **e-mail normalizado** (mesma normalização da importação) + fallback por telefone
- **Regra de segurança**: se o vínculo for ambíguo (2+ leads com o mesmo e-mail), a IA NÃO envia — vira rascunho com alerta de conflito para o owner
- A conversa lida é guardada em `ia_envios.conversa_resumo` (trechos citados que provam a dor)

### F1.3 — Geração do e-mail (IA nativa Skip)
- Prompt com: resumo da conversa (trechos reais), produto/família, especialista do cadastro, horários do cadastro, vídeo/business case do cadastro
- **Prova de entendimento da dor**: só com trechos da conversa real (citação) — se a conversa não tem dor clara, o e-mail não sai (transbordo)
- **Perguntas de qualificação**: 1 a 3, vindas das regras por produto (cadastro F0.1)
- **Horários**: 2 ou 3 do cadastro F0.3, no fuso do lead
- **Vídeo/business case**: só se o cadastro tiver para aquele produto e a dor casar
- **Especialista**: nome do cadastro F0.1 — nunca inventado
- Assinatura em nome do CEO (texto fixo aprovado pelo owner no cadastro)

### F1.4 — Envio Microsoft 365
- Mesma esteira do relatório diário: app registration com Mail.Send + Application Access Policy restrita à caixa de envio
- Falha de envio → status em `ia_envios` + alerta; **nunca reenviar automaticamente** (risco de duplicar contato com cliente)

### F1.5 — Registro no CRM
- O e-mail enviado vira interação no lead (tipo "e-mail IA", com link para `ia_envios`)
- Resposta do cliente entra como interação normal (o vendedor vê o histórico completo)

## Pré-mortem do Bart (5 jeitos de quebrar em produção — com prova planejada)

1. **E-mail em nome do CEO com dado errado** (dor inventada, especialista errado, horário impossível) → prova: modo rascunho obrigatório no início; cada rascunho conferido contra a conversa real; varredura de dado inventado no e-mail gerado (todo trecho de dor deve existir na conversa)
2. **Vínculo errado de conversa** (e-mail do lead A com conversa do lead B) → prova: caso ambíguo → NÃO envia, vira rascunho com alerta; caso único → vínculo correto conferido
3. **Dependência do Envya derrubar o fluxo** (API fora/timeout/mudança de formato) → prova: falha simulada → o lead NÃO perde o registro (fica pendente com status de erro), alerta ao owner, retry manual — nunca silencioso
4. **Duplicação de contato** (RD reenvia o webhook; lead pontuado 2×) → prova: mesmo webhook 2× → 1 só e-mail (idempotência por lead+gatilho)
5. **Webhook público vira porta de entrada** (spam/forgery) → prova: sem secret → 401; payload inválido → 400; rate limit; nada grava sem secret válido

## Critérios de aceite (provas do Bart)

1. F0: cadastros CRUD só admin; IA lê server-side; regras de roteamento editáveis sem deploy; horários nunca inventados no e-mail
2. Modo rascunho: nenhum e-mail sai sem clique humano; fila visível ao owner; auditoria completa em `ia_envios`
3. F1 (quando destravado): webhook com secret; vínculo ambíguo não envia; e-mail com citações reais da conversa; falha do Envya não perde lead; idempotência de gatilho
4. Regressão por papel em todas as telas + varredura de dado inventado nos e-mails gerados
5. Logs limpos após a rodada de provas

## Ordem de implementação sugerida

1. **F0 completo** (sem pendências — CC pode começar quando autorizado)
2. Owner resolve pendências (APIs, especialistas, vídeos, Paulo, M365)
3. **F1.1–F1.2** (webhook + vínculo) em modo rascunho
4. Owner valida rascunhos por 1–2 semanas
5. **F1.3–F1.5** (geração + envio) — modo automatica só depois da validação

## Nota sobre o Bloco D

Importação do Pipedrive **suspensa pelo owner** (histórico não confiável) — nada foi importado; a rota e a tela ficam prontas para quando ele reavaliar. Sem impacto no F0/F1.
