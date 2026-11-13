# Revisão do Bloco 3 — provas do Bart (0.0.67)

**Data**: 13/11/2026 · **Veredito**: BLOQUEADO — defeito D3 bloqueante; regras de papel corretas (11 provas verdes).

## D3 (bloqueante): leitura e exclusão autenticadas quebram em TODAS as coleções

**Sintoma**: GET (list/view) e DELETE com auth → 400 "Something went wrong while processing your request." em TODAS as coleções (leads, produtos, users, tickets, oportunidades…). Sem auth, GET responde 200 com lista vazia (regra exige auth — correto). POST e PATCH (hooks de MODEL) funcionam 100%.

**Padrão que isola a causa**: hooks de REQUEST quebram; hooks de MODEL funcionam. Coleções SEM hook novo (tickets) também quebram → o problema está no hook `permissoes_papel.js` registrando callbacks de request que estouram para QUALQUER requisição autenticada (exceção interna vira 400 genérico antes do `e.next()`).

**Suspeito principal**: `e.hasSuperuserAuth()` estourando no contexto JSVM quando a requisição é de usuário comum — ou leitura de `e.auth.collection().name`/`papel` em caminho nulo.

**Comportamento temporal**: as primeiras provas de GET passaram (5/5); após a primeira falha, ficou permanente (20/20 falhas; esperas de 30s/60s/2min/5min/10min não recuperaram).

**Impacto**: a TELA usa o mesmo backend — listagens quebradas = plataforma inutilizável para admin e vendedor. Julio e Rose não conseguem usar nada enquanto isso persistir.

**Ação pedida**: revisar `permissoes_papel.js` — envolver o corpo de cada callback de request em try/catch defensivo, ou substituir `e.hasSuperuserAuth()` por verificação segura (`e.auth !== null && e.auth.collection().name === 'users' && e.auth.getString('papel') === 'admin'`), garantindo que NENHUM caminho estoure antes do `e.next()`. O mesmo vale para os hooks de view/list de auditoria. Reimplantar e avisar — o Bart re-roda a bateria completa.

## Provas que PASSARAM (antes do bloqueio — todas as regras de papel funcionam)

1. (a) Cadastro sem login → 400 ✓
2. (b) Admin tenta apagar lead real (Aroldo) → 403 ✓ (proteção do Bloco 2 vale para admin)
3. (d) Renomear lead real para "PROVA …" → 400 ✓
4. (e) Vendedor cadastra lead da empresa "Demolidora" → aceito ✓ (palavra inteira correta)
5. Vendedor apaga lead PROVA → 403 ✓; admin apaga PROVA → 204 + auditoria com `apagado_por_tipo: "administrador"` ✓
6. Vendedor lê auditoria → 403 ✓; lê fechamentos → 403 ✓; consulta auditoria por id → 404 (exceção documentada) ✓
7. Vendedor cria usuário → 403 ✓; troca o próprio papel → 403 ✓; edita outro usuário → 404 (exceção documentada) ✓
8. Vendedor edita produto → 403 ✓; último admin não rebaixa a si → 400 ✓
9. Vendedor cria lead DEMO → 403 ✓; admin cria DEMO → OK ✓
10. Vendedor edita lead próprio → 200 ✓ (opera o dia a dia)
11. Admin lista produtos e edita produto → OK (nas janelas em que o servidor respondia) ✓

**Observação de método**: nas primeiras rodadas usei o header `Authorization` sem `Bearer` em GET — isso também produz 400 genérico no PocketBase. Re-provei tudo com `Accept: application/json` + header correto; o padrão POST/PATCH-OK vs GET/DELETE-400 se manteve com tokens frescos e headers corretos, confirmando que o D3 é do servidor, não do cliente.

## Sinal do teste com usuários (repasse)

4 tentativas seguidas (15:35) de mudar de etapa sem ambiente e valor na mesma oportunidade (id `ha45pnwyyzifxqy`) — o aviso "Próximo passo" talvez precise de mais destaque. Confirmar com Julio/Rose na sessão; se confirmarem, ajuste de UI (destaque visual do aviso de bloqueio).

## Limpeza pendente (após correção do D3)

Leads de prova criados durante as provas: `PROVA B3 Ven…VenB` (maioria já apagada), `Contato B3…B3f` empresa "Demolidora Comércio" (arquivados com motivo — não apagáveis por não serem PROVA), 1 lead DEMO. O lead real Aroldo está íntegro (a prova b foi só tentativa de DELETE recusada).

## Conclusão

As regras de papel do Bloco 3 estão corretas (11 provas verdes), mas o D3 torna a versão 0.0.67 inutilizável na tela. **Não aprovar o bloco até a correção.** Prioridade máxima.
