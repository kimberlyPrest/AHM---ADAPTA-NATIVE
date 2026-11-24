# Revisão do Bloco Segurança da equipe (0.0.79) — provas do Bart · 24/11/2026

**Estado conferido em 24/11 15:50: versão 0.0.79, última migration 0039, working tree limpo.** Protocolo aplicado: pré-mortem, regressão por papel, varredura, dado inventado = defeito, logs, riscos além do pedido.

## Resultado: APROVADO COM 1 DEFEITO (B1) + 1 observação — o resto, 100% verde

## Provas do pedido (API + tela)

| Prova | Resultado |
|---|---|
| P1: usuário PROVA vendedor criado; login funciona antes do bloqueio | ✅ |
| P2: bloquear sem motivo → 400 "Informe o motivo do bloqueio." | ✅ |
| P2: bloquear com motivo → 200; `bloqueado_em`/`bloqueado_por` gravados | ✅ |
| P3: login bloqueado → recusado (403 da authRule) | ✅ |
| **P3b: token antigo após bloqueio → requisições normais AINDA passam (200)** | ❌ **B1** |
| P4: reativar → 200; motivo/data/autor limpos; login volta | ✅ |
| P5: vendedor bloqueia outro → recusado (404 da regra de update — vendedor não edita colega) | ✅ |
| P6: admin bloqueia a si → 400 "Você não pode bloquear o seu próprio acesso." | ✅ |
| P6: bloquear o único admin ativo → 400 (provado com cenário de 1 admin; hoje há 2 — Afonso e Kimberly) | ✅ |
| P7: redefinir-senha — vendedor → 403 ✅ · id inexistente → 404 ✅ · bloqueado → 400 ✅ · ativo → 200 com mensagem clara ✅ | ✅ |
| P8: alterar minha senha — atual errada → 400 ✅ · nova curta → 400 ✅ · sucesso → 200 ✅ · antiga recusada ✅ · nova funciona ✅ | ✅ |
| P9: Beatriz/Carlos/Mariana bloqueados com motivo ✅ · login com senhas comuns recusado (bloqueio vale no login) ✅ | ✅ |
| Tela Equipe: 3 exemplos com "Acesso bloqueado — <motivo>" ✅ · botão "Enviar link de senha" ✅ · bloquear/reativar ✅ | ✅ |
| Regressão: vendedor troca papel → recusado ✅ · vendedor cadastra usuário → recusado (400 genérico — nota D4 conhecida) ✅ · Bloco R1 intacto (Eduardo "em negociação") ✅ | ✅ |
| Linguagem pt-BR nas mensagens novas | ✅ |

## DEFEITO B1 (médio): bloquear NÃO encerra as sessões já abertas na prática

- **Reprodução**: login do usuário PROVA → token T1 → bloquear como admin (200, hook roda `refreshTokenKey()`) → requisição com T1 → **200** (deveria ser 401). Repetido em 2 ciclos — mesmo resultado.
- **O que funciona**: login bloqueado → recusado ✅; `auth-refresh` com o token antigo → 401 ✅ (a renovação é barrada).
- **Causa provável**: o `refreshTokenKey()` roda no request hook, mas o PocketBase valida o JWT das requisições normais pela assinatura/expiração — não confere o `tokenKey` atual do registro em cada chamada. O token antigo segue válido até expirar.
- **Impacto**: quem estiver logado quando for bloqueado continua operando até o token expirar (janela do token). O bloqueio impede NOVO login imediatamente — o pior caso (senha pública dos exemplos) está coberto, porque quem não tem sessão não entra.
- **Correção sugerida ao CC**: verificar como o PocketBase invalida tokens ao trocar `tokenKey` (pode exigir gravar o novo tokenKey de forma diferente no hook, ou aceitar a limitação e documentá-la). Se não houver caminho no JSVM, registrar como limitação conhecida e reduzir o tempo de vida do token.

## Observação (baixa)

- **Vendedor cadastrando usuário → 400 genérico** ("Failed to create record.", sem mensagem corporativa) — é o mesmo padrão do D4 do Bloco 3 (API rule falha antes do hook). Nada foi criado (conferido: 0 registros). Padronização futura.

## Decisões que ficam com o owner (da entrega, confirmadas)

1. **Rose Teste / Rose Teste 2**: contas de teste ativas e desbloqueadas. Sugestão: manter Rose Teste 2 (e-mail real `teste.rose2@`), bloquear Rose Teste e trocar a senha da mantida.
2. **Kimberly como Administradora**: existe e está ativa — confirmar se é a intenção (hoje são 2 admins: Afonso e Kimberly).
3. **Julio (`julio@ahmsolution.com`)**: sem e-mail visível na listagem (`emailVisibility=false`) — se essa caixa não recebe e-mail, o link de senha não chega. Conferir.
4. **Remetente dos e-mails**: `noreply@mail.goskip.dev` (limite 50/h) — pode cair no spam; remetente próprio é configuração futura.

## Logs e limpeza

- Nenhum warning/erro novo de hook nos meus testes (além dos 400/403 esperados).
- **Limpeza**: usuário PROVA excluído (204); nenhum registro de prova restante; base de leads intacta (15.406).

## R1–R10

| Req | Situação |
|---|---|
| R3 Permissão mínima | ✅ bloqueio/reativação/redefinição só admin (403 provados) |
| R5 Auditoria | ✅ bloqueado_em/por/motivo gravados; limpos na reativação |
| R8 Erros | ✅ mensagens pt-BR claras em todos os casos novos |
| R2 Regra no servidor | ✅ authRule na coleção — login bloqueado recusado por qualquer origem |

## Conclusão

**BLOCO SEGURANÇA DA EQUIPE: APROVADO COM B1 PENDENTE** (sessões abertas não são encerradas na prática; novo login e renovação sim). O risco original (senhas dos exemplos publicadas no repo) está **resolvido**: os 3 usuários estão bloqueados com senha aleatória e login recusado.

**Próximo passo**: CC investiga/corrige B1 (ou documenta como limitação) → eu re-provo o cenário do token antigo → "pode concluir".

## ADENDO 1 — re-prova na 0.0.80 + correção do e-mail do Julio (24/11, 17:50)

**Estado conferido em 24/11 17:50: versão 0.0.80, migration 0040, working tree limpo.**

| Prova | Resultado |
|---|---|
| B1 re-prova: token antigo após bloqueio → deveria ser 401 | ❌ **B1 CONTINUA** (200 — o hook é o mesmo da 0.0.79; a 0.0.80 não tocou no B1) |
| Migration 0040: e-mail do Julio → julio.marcos@ahmsolution.com, emailVisibility=true, verified=true, MESMO id (históricos preservados) | ✅ |
| Login do Julio com o e-mail novo | ✅ funciona |
| Renovação de sessão (auth-refresh) e requisições normais do Julio | ✅ |
| **Senha de teste do Julio (Teste@CRM2026) NÃO foi trocada — login com ela ainda funciona** | ⚠️ risco pendente do owner (as senhas foram enviadas por e-mail em 13/11) |

**Leitura do B1**: a 0.0.80 corrigiu o e-mail do Julio (era isso que travava o link de senha); o B1 (sessões abertas não encerradas na prática) segue em aberto — ou o CC o resolve com um mecanismo diferente (ex.: middleware global que recusa requisitante bloqueado), ou o registramos como limitação conhecida com o tempo de vida do token reduzido. O pior caso (senhas públicas dos exemplos) permanece coberto: login novo é recusado.

**Rose Teste / Rose Teste 2 (pergunta do owner)**: o Bart **não usa nenhuma** nas provas — usa o Julio. As duas estão hoje desbloqueadas; a Rose Teste 2 tem e-mail real (teste.rose2@), a Rose Teste não tem e-mail visível. Pode bloquear as duas; se quiser manter uma para testes próprios, mantenha a Rose Teste 2.
