# Re-prova do débito de tela — sessão encerrada com aviso (0.0.83) · Bart · 24/11/2026 19:05

**Contexto:** débito de UX da re-prova do B1 — bloqueado com aba aberta via telas de "base vazia". CC corrigiu na 0.0.83: ao trocar de página ou voltar à aba, a sessão é conferida no servidor; se foi encerrada, aviso + login.

## Resultado: **CORRIGIDO — 3/3 provas PASS**

| Prova | Resultado |
|---|---|
| Aba aberta como PROVA Vendedor → bloqueio via API → clicar em "Oportunidades" | ✅ aviso **"Sua sessão foi encerrada — Seu acesso foi alterado (senha trocada ou acesso bloqueado). Entre novamente"** + redirecionamento ao login na primeira navegação |
| Recarregar a página com o token morto | ✅ direto ao login, sem vazar conteúdo |
| Reativação da conta | ✅ login volta a funcionar (200) |

Estado final: 10 usuários — PROVA Vendedor (Bart) **ativa**; Rose Teste e Rose Teste 2 bloqueadas ("Conta de teste"); Beatriz/Carlos/Mariana bloqueadas; Julio, Rose e Kimberly ativos. Base intacta: 15.406 leads, zero lixo de prova.

## Correção histórica (owner, 24/11 18:48)

O owner esclareceu: **nunca fez o bloqueio das Rose Teste nem criou a conta PROVA** — o texto da mensagem anterior dizia "bloqueadas/criei" por erro de redação. **Não há outro banco.** A frase do relatório anterior ("provavelmente feito em banco não publicado ou não aplicado") estava errada e foi corrigida nele. O bloqueio e a conta foram executados pelo Bart sob autorização explícita (24/11 18:22).

## Riscos que encontrei além do pedido (protocolo de revisão)

1. **(NOTA DE DESENHO, não defeito) Vendedor enxerga TODAS as leads.** A PROVA Vendedor — dona de zero leads — viu a lista de Leads com a base inteira (15.406). Causa: regra da coleção `leads` desde a Fase 1 é "qualquer usuário logado" (`@request.auth.id != ''`, migration 0014); "Minha mesa" filtra por `responsavel`, a tela de Leads não. Não é regressão da 0.0.83 e o Bloco 3 revisou exatamente o oposto (vendedor precisava ver colegas em `users`). Se o isolamento por vendedor for desejado, é decisão de negócio do owner (portão dele) — registrado para o dia da entrada em uso pelo time.
2. **(BAIXA, persiste) Erro de escrita do bloqueado segue genérico** ("Failed to create record.") — coberto pelo R8 do contrato no passo 2 (campos de auditoria + erros com código).

## Conclusão

Débito de tela corrigido e provado. Com isso, o **Bloco Segurança da equipe está pronto para o "pode concluir" do owner** (B1 provado na 0.0.82 + débito de tela provado na 0.0.83; resta o erro genérico como débito pequeno endereçado pelo passo 2 do contrato).
