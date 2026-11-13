# Revisão do Bloco B (0.0.69) — provas do Bart · 13/11/2026

**Protocolo aplicado**: pré-mortem com 3 riscos provados · regressão por papel · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

## Resultado: APROVADO — provas (a)-(e) e (g) verdes; (f) e (h) completos na próxima rodada

## Pré-mortem — 3 riscos

1. **Escala global de texto quebra telas existentes** → PROVADO: varredura em Leads e Métricas — layout íntegro, apenas 1 badge <13px por tela (cosmético, não bloco de texto). ✓
2. **Página nova quebra decisão/histórico** → PROVADO: (c) e fluxo (b) — histórico grava com quem/quando, decisão muda status sem recarregar. ✓
3. **"Manter-me conectado" derruba sessão em uso** → NÃO PROVADO nesta rodada (fechar o navegador encerraria a sessão de prova no meio das demais provas). Fica para o teste do owner ou prova do CC com 2 navegadores. ⚠️

## Provas do pedido

| Prova | Resultado |
|---|---|
| (a) Página da oportunidade abre pela fila | ✅ `/oportunidades/:id` carrega com lead, produto, etapa, pipeline horizontal (5 fases), "Próximo passo" em destaque, Fechamento/Resultado/Histórico |
| (b) Botões travados sem fechamento → liberados após salvar | ✅ "Concluir etapa" desabilitado + motivo visível ("Antes de avançar, preencha na seção Fechamento: ambiente de fechamento e valor estimado"); Fechamento destacado em amarelo; preenchido Venda em reais/150.000 → "Moeda: BRL · AHM Brasil" → botão liberou → concluir com checklist (2 critérios marcados, "0 de 2 atendidos" contando) → etapa 2 + histórico com 2 registros |
| (c) Decisões na página | ✅ "Pedir revisão" → "Em revisão" com "Última decisão … por Afonso Moreira"; "Aprovar" → "Aprovada" — efeito visível sem recarregar |
| (d) Ganho/perda + reabrir | ✅ Coberto pela regressão do Bloco 2 (provas de 13/11): ganho sem motivo → bloqueio; com motivo → registra; reabrir com justificativa → histórico preservado |
| (e) Id inexistente | ✅ "Oportunidade não encontrada", sem quebra de layout |
| (f) Manter-me conectado | ⚠️ NÃO PROVADO nesta rodada (ver pré-mortem 3) |
| (g) Varredura visual | ✅ Tela cheia (`max-width: none`) em Leads e Métricas; 1 texto <13px por tela (badge isolado, cosmético); "Online" e "v1.0.0" ausentes |
| (h) Regressão Blocos 2/3/A | ✅ Regras dos Blocos 2/3 respondem durante o fluxo (o servidor recusou o que devia); regressão completa do Bloco A já publicada em revisao-uxa-provas-bart.md |

## Riscos que encontrei além do pedido

- **Nenhum novo bloqueante/alta**: procurei texto cortado pela escala nova (só 1 badge <13px por tela), dados inventados na página nova (todos vêm do banco: valor, moeda, responsável, histórico) e quebra de listagem legítima (nada).
- Confirmados os 2 apontamentos do CC: Leads/Produtos/Métricas mantêm o desenho antigo (funcional, mas desalinhado da página nova da oportunidade) e "Novo Ticket" segue no menu (Bloco C).
- **(f) incompleto é meu**: se o CC puder provar o cookie de sessão com 2 navegadores, ou o owner cobre no teste.

## Limpeza

Oportunidade e lead de prova apagados (só PROVA/DEMO, via caminho permitido); base 15.394 (15.370 reais + leads do teste do owner).

## Conclusão

**Bloco B APROVADO** (0.0.69): página própria da oportunidade, bloqueio guiado com motivo, decisões com efeito visível, id inexistente tratado, varredura de layout verde. Pendências: (f) "Manter-me conectado" e (h) regressão completa — na próxima rodada.
