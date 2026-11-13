# Revisão do Bloco 3 (0.0.67) — provas do Bart · 13/11/2026

## Resultado FINAL: APROVADO — 23/24 provas verdes; o "D3" era falso positivo do script de provas

**Correção de diagnóstico (13/11 ~15:40)**: o 400 "Something went wrong" em GET/DELETE autenticados NÃO era defeito do servidor — era o helper do meu script enviando o token como CORPO da requisição (argumento posicional caiu no parâmetro body, com Content-Type: application/json). O PocketBase lê o corpo antes de regras e hooks e recusa JSON malformado com esse 400 genérico (evidência do CC: os 400 levavam 0,03–0,25 ms — rápido demais para hook/banco; GET sem corpo → 200; GET com corpo JSON inválido → 400 mesmo sem login). Nada foi alterado no Skip; a versão segue 0.0.67. Aprendizado registrado: em GET/DELETE, nunca enviar corpo nem Content-Type; token sempre como keyword.

## Bateria completa re-executada com o script corrigido — 23/24 PASS

1. Sanidade: GET admin sem corpo → 200 ✓
2. (a) Cadastro sem login → 400 ✓
3. Vendedor cria lead PROVA → 200 ✓
4. (c1) Vendedor apaga PROVA → 403 ✓
5. (c2) Admin apaga PROVA → 204 ✓
6. (c3) Auditoria registra `apagado_por_tipo: "administrador"` ✓
7. (b) Admin apaga lead real (Aroldo) → 403 ✓ (proteção do Bloco 2 vale para admin)
8. (d) Renomear lead real para "PROVA …" → 400 ✓
9. (e) Vendedor cadastra "Demolidora …" → 200 ✓ (palavra inteira correta)
10. Vendedor edita lead próprio → 200 ✓ (opera o dia a dia)
11. Vendedor lê auditoria → 403 ✓
12. Vendedor lê fechamentos → 403 ✓
13. Vendedor consulta auditoria por id → 404 ✓ (exceção documentada)
14. Vendedor troca o próprio papel → 403 ✓
15. Vendedor edita outro usuário → 404 ✓ (exceção documentada)
16. Admin lista produtos → 200 ✓
17. Vendedor edita produto → 403 ✓
18. Admin edita produto → 200 ✓
19. Último admin não rebaixa a si → 400 ✓
20. Vendedor cria lead DEMO → 403 ✓
21. Admin cria lead DEMO → 200 ✓
22. Admin apaga DEMO → 204 ✓
23. Arquivar lead de teste "Demolidora …" (limpeza) → 200 ✓

## Nota D4 (menor, não bloqueante): cadastro de usuário por vendedor responde 400, não 403

`POST /api/collections/users/records` como vendedor → 400 "Failed to create record." (sem mensagem corporativa). Causa: a API rule `create: @request.auth.papel = 'admin'` falha ANTES do hook 5b — o PocketBase responde rule de create falhada com 400 genérico, não 403. O resultado funcional está correto (não-admin não cria usuário; admin cria → 200 com papel default "vendedor" ✓). Sugestão para padronização futura: mover a regra para o hook (rule permissiva + hook lança ForbiddenError com mensagem corporativa) — mesma técnica das demais coleções. Não bloqueia o bloco.

## Sinal do teste com usuários (repasse)

4 tentativas seguidas (15:35) de mudar de etapa sem ambiente e valor na mesma oportunidade (id `ha45pnwyyzifxqy`) — o aviso "Próximo passo" talvez precise de mais destaque. Confirmar com Julio/Rose na sessão; se confirmarem, ajuste de UI (destaque visual do aviso de bloqueio).

## Limpeza

Leads de prova das rodadas anteriores: `PROVA B3 Ven…VenB` (apagados), `Contato B3…B3f` empresa "Demolidora Comércio" (arquivados com motivo), 1 lead DEMO (apagado), usuário de teste da regra (criado e apagado). Lead real Aroldo íntegro. Base real intacta.

## Conclusão

**Bloco 3 APROVADO pelo Bart** — permissões por papel implementadas corretamente (0.0.67, migration 0031). Liberado para o uso do time; teste com usuários de Julio e Rose pode prosseguir.
