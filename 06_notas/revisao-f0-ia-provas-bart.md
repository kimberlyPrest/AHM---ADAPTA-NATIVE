# Revisão da F0.1–F0.5 da IA de vendas (0.0.74) — provas do Bart · 14/11/2026

**Protocolo aplicado**: pré-mortem com 3 riscos provados · regressão por papel · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

## Resultado: APROVADO — provas de API e tela verdes; 1 observação de validação (janela com dia fora de 0-6 gravou dia 0) e 1 nota de segurança (PATCH/DELETE em ia_envios com id inexistente devolve 404, não 403)

## Contexto

F0.1–F0.5 = fundação da IA de vendas SEM envio de e-mail: base de conhecimento com fonte obrigatória, roteamento por modelo de negócio, janelas de atendimento, fila de envios imutável, modo de operação (começa em rascunho). Tela `/ia` só admin. Migration 0035 + hook `ia_fundacao.js` + rota `GET /backend/v1/ia/horarios-previa` + `src/lib/horarios.ts` (6 testes do CC).

## Provas do pedido

| Prova | Resultado |
|---|---|
| (a) Admin forja POST em `ia_envios` → 403 | ✅ "Os registros de envio da IA são gravados apenas pelo sistema." — nem admin grava; só o servidor (`$app.save`) |
| (b) Vendedor edita regra → 403 | ✅ "Esta configuração só pode ser alterada por um administrador." · Vendedor escreve na base de conhecimento → 403 · Vendedor LISTA base e envios → 403 nos dois |
| (c) Ativar regra do Paulo sem vendedor → 400 | ✅ "Para ativar a regra de roteamento, escolha o vendedor." · Escolher vendedor limpa `aguardando_cadastro` (prova: Julio → aguardando false) · Restaurado (inativo, sem vendedor) |
| (d) Prévia de horários vazia c/ mensagem | ✅ `{"janelas":0,"horarios":[],"erro":"agenda_indisponivel","mensagem":"A agenda do Outlook ainda não está conectada. Nenhum horário é oferecido ao cliente até a conexão existir."}` · Sem login → 401 · Vendedor → 403 "A prévia de horários é restrita ao administrador." |
| Fonte vazia na base → 400 | ✅ "Informe a fonte deste conhecimento: sem fonte, a IA não pode usá-lo." · Com fonte → 200 (registro de prova criado e apagado) |
| Janela início ≥ fim → 400 | ✅ "O horário de início da janela precisa ser anterior ao horário de fim." |
| `ia_modo` inválido → 400 | ✅ "Modo de operação inválido. Use desligada, rascunho ou automática." · Chave da configuração não pode mudar → 400 ✓ |
| Modo rascunho ↔ automática | ✅ Troca funciona (prova: rascunho → automática → rascunho); estado final = **rascunho** (o recomendado) |

## Tela `/ia` (como admin, browser)

- Cabeçalho explica: "Nesta fase nenhum e-mail é enviado" — coerente com a fase
- **Modo de operação**: 3 botões (Desligada / Rascunho · ativo / Automática) com descrições claras; "Rascunho (recomendado no início) · ativo" correto
- **Fila de envios**: "Nenhum envio ainda. Os primeiros rascunhos aparecem aqui quando a entrada inteligente for ligada." — sem dado inventado ✓
- **Roteamento**: tabela com os 5 modelos — Assinatura→Julio (Ativa), Cíclico→Julio (Ativa), Pontual→"Paulo (aguardando cadastro)" (Sem regra: vai para o administrador), Locação e Consultoria→Sem executivo (Sem regra: vai para o administrador) — exatamente a decisão do owner ✓
- **Janelas de atendimento**: seletor de executivo + explicação "dentro da janela E livre na agenda do Outlook" ✓
- **Base de conhecimento**: "A base ainda está vazia. Comece pelos especialistas de cada produto e pelos vídeos e business cases." + botão "Cadastrar conhecimento" ✓
- Menu com "IA de vendas" (só admin)

## Pré-mortem — 3 riscos

1. **Fila de envios editável** (quebra a auditoria P3) → PROVADO: POST admin → 403; a coleção só aceita gravação do servidor. ✓
2. **Roteamento ativo sem vendedor** (lead perdido na fila errada) → PROVADO: 400 ao ativar sem vendedor. ✓
3. **Dado inventado na tela** → PROVADO: fila vazia diz "Nenhum envio ainda" (real); roteamento reflete as regras do banco; base vazia diz que está vazia. ✓

## Riscos que encontrei além do pedido

1. **(baixa) Janela com dia fora de 0-6 gravou como dia 0**: meu POST com `dia:9` (campo errado do meu teste — o campo real é `dia_semana`) foi aceito com 200 e gravou `dia_semana: 0` (domingo). O hook valida `dia_semana` 0-6, mas o campo `dia` desconhecido foi ignorado silenciosamente pelo PocketBase e o registro entrou. **Não é o cenário que o CC descreveu** (validação de dia_semana existe — não re-testei com o campo correto). Apaguei o registro órfão. Recomendo re-teste do CC com `dia_semana: 9` → 400.
2. **(baixa) PATCH/DELETE em `ia_envios` com id inexistente → 404**, não 403 — a regra da coleção (list rule) barra antes do hook (mesmo caso do D4 do Bloco 3). O POST (o ataque real de forjar envio) responde 403 com a mensagem correta. Comportamento seguro (nada editável), só a mensagem difere.
3. **(nota) Cópia gêmea da função de horários** dentro do hook (JSVM não aceita require local) — o CC registrou nos dois arquivos que mudanças devem ser feitas nos dois. Risco de divergência futuro; sugerir teste de consistência quando a F0.6 conectar a agenda.
4. **(nota) Locação e Consultoria sem regra** → fila do admin, visível na tela. Se o owner quiser rotear (ex.: Locação → Rose), é 1 clique na tela.

## Segurança

- Base de conhecimento e `ia_envios`: leitura e escrita só admin (403 provados para vendedor) ✓
- Prévia de horários: só admin (401 sem login, 403 vendedor) ✓
- Sem login: recusas em português (conferidas) ✓
- Nenhum e-mail é enviado nesta fase — nada para vazar ✓

## Logs

- Nenhum warning/erro novo de hook (último e.next() segue 12/11 12:53) ✓

## Limpeza / estado final

- Registro de base de conhecimento de prova: criado e apagado ✓
- Janela órfã do teste (dia 9 → dia 0): apagada ✓
- Regra do Paulo: restaurada (inativa, aguardando cadastro, sem vendedor) ✓
- Modo: restaurado para **rascunho** ✓
- Base de conhecimento: 0 registros; janelas: 0 registros; `ia_envios`: 0 registros ✓

## Conclusão

**F0.1–F0.5 APROVADA** (0.0.74): fundação sólida — auditoria da fila de envios impenetrável (nem admin), roteamento com validação, janelas com prévia honesta (agenda não conectada → nenhum horário oferecido), base com fonte obrigatória, modo começa em rascunho. Pendências para F0.6/F1: admin M365, API RD/Envya, especialistas, materiais, cadastro do Paulo.
