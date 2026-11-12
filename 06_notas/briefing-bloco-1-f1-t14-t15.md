# Briefing — Bloco 1: F1-T14 + F1-T15 (SPEC 1.5 — decisões na oportunidade)

**Quem executa:** Claude Code (sessão AHM, via MCP Skip) — escreve e roda o QA do Skip.
**Quem revisa:** Bart (Maestro) — provas via API, validação de tela, docs e GitHub. NÃO edite fase.md/STATUS/changelog — isso é do Bart.
**Coordenação:** um escritor por vez. Bart não toca nos arquivos do Skip enquanto você implementa. Ao terminar, o webhook avisa o Bart automaticamente.

## Contexto (o que JÁ existe — não recriar)

- Coleção `oportunidades` (migration 0019): lead, produto, justificativa, confianca (alta|média|baixa), status (Sugerida|Aprovada|Rejeitada|Em revisão), comentario, criado_em, atualizado_em + índices.
- Hook `pocketbase/hooks/on_leads_oportunidade.js` (RN-1.13): lead qualificado cria "Sugerida"; oportunidade aberta do mesmo lead+produto é ATUALIZADA (nunca duplicada); Aprovada/Rejeitada não reabre; falha não bloqueia qualificação.
- Tela `src/pages/Oportunidades.tsx`: fila paginada server-side, chips de status, contadores (Total/Sugeridas/Aprovadas), modal com justificativa + confiança + contato do lead + situação do lead. SEM botões de decisão (era o recorte da F1-T13).
- Versão corrente do projeto: 0.0.45. Base real: NÃO apagar o lead Eduardo Zuin/Arkema, os 4 leads qualificados, a oportunidade real e os 2 transbordos reais.

## O que implementar

### F1-T14 — Decisões aprovar/rejeitar/revisar (CA-1-014)

1. **Migration 0020** (numeração ordinal de 4 dígitos, up/down completo): campos novos na coleção `oportunidades`:
   - `decidido_em` (datetime, opcional)
   - `decidido_por` (relation → users, opcional)
2. **Hook de auditoria** (`onRecordUpdateRequest` em `pocketbase/hooks/` — request hook, TEM `e.auth`): quando o status transiciona para Aprovada/Rejeitada/Em revisão e `decidido_por` está vazio, preencher `decidido_em`/`decidido_por` automaticamente.
3. **Hook de validação** (`onRecordUpdate`): transição para Rejeitada exige `comentario` preenchido (RN-1.14 — motivo obrigatório na rejeição). Transição de Aprovada de volta para Em revisão (rollback da SPEC) exige `comentario` (justificativa da volta).
4. **Tela** (`src/pages/Oportunidades.tsx`), no modal de detalhe:
   - Item "Sugerida" ou "Em revisão": botões **Aprovar**, **Pedir revisão** e **Rejeitar**.
   - "Pedir revisão" e "Rejeitar" abrem campo de comentário (obrigatório só na rejeição; na revisão é opcional mas incentivado — placeholder orienta).
   - Item "Aprovada": botão **Voltar para revisão** com justificativa obrigatória (rollback auditável).
   - Após a ação: toast de confirmação em português correto ("Oportunidade aprovada", "Oportunidade rejeitada", "Oportunidade em revisão") e fila recarregada.
   - Modal passa a mostrar "Decidido em <data> por <nome>" quando `decidido_em` existir (expand em decidido_por).
   - Contadores: acrescentar card "Rejeitadas" (o service `contadores()` em `src/services/oportunidades.ts` precisa retornar também rejeitadas/em revisão — ajustar).

### F1-T15 — Motivo obrigatório + duplicidade sinalizada (CA-1-015)

1. O bloqueio de rejeição sem motivo já sai do hook do F1-T14 — **provar** que a UI também impede (botão desabilitado sem texto) e que a API rejeita com 400 e mensagem corporativa.
2. Duplicidade: o hook RN-1.13 já atualiza em vez de duplicar. Acrescentar nele: ao ATUALIZAR uma oportunidade aberta existente, gravar em `comentario` (sem apagar o anterior — concatenar em nova linha): "Justificativa atualizada pela requalificação do lead em <data ISO>." — é o sinal de duplicidade tratada.
3. Prova de limite: requalificar um lead com oportunidade aberta → justificativa atualiza, comentario acumula, NÃO cria segunda oportunidade.

## Regras obrigatórias (do CLAUDE.md — não negociável)

- **Linguagem**: todo texto de usuário em pt-BR corporativo — maiúscula inicial, pontuação completa, plural correto ("1 item", "2 itens"). NUNCA expor códigos internos (RN-x.x, SPEC, F1-Txx) em strings de UI, toasts ou mensagens de erro — só em comentários de código.
- **Migrations**: nova migration para cada mudança de schema (0020), com down que desfaz. Nunca editar migration aplicada.
- **Hooks**: validação de dados em model hook (`onRecordCreate/Update`); auditoria "quem fez" em request hook (`onRecordUpdateRequest`); efeito pós-commit em `onRecordAfterUpdateSuccess`. Todo caminho termina em `return e.next()` ou `throw new BadRequestError(...)`.
- **Paginação server-side** sempre (a fila já é); não introduzir getFullList.
- **QA**: `skip_project_apply_changes` precisa passar (lint + build). Build quebrado = corrigir antes de encerrar.
- **Base real**: não apagar registros reais (Eduardo Zuin/Arkema, 4 leads qualificados, 1 oportunidade, 2 transbordos). Provas com registros de teste são do Bart — você não precisa criar/apagar dados, só implementar.

## Definition of Done (o Bart valida isto)

- [ ] Migration 0020 aplicada (up/down OK).
- [ ] Aprovar/Rejeitar/Pedir revisão funcionam na tela e gravam decidido_em/decidido_por.
- [ ] Rejeição sem motivo: bloqueada na UI (botão desabilitado) e na API (400 com mensagem corporativa).
- [ ] Rollback Aprovada → Em revisão exige justificativa.
- [ ] Requalificação de lead atualiza oportunidade aberta (sem duplicar) e acumula comentário de atualização.
- [ ] Contadores com Rejeitadas visíveis.
- [ ] QA do Skip verde (lint + build).
- [ ] Zero códigos internos em strings de usuário; pt-BR correto em tudo que aparece na tela.

Ao terminar: rode o apply_changes (isso já avisa o Bart pelo webhook) e NÃO conclua a task — a conclusão é do Bart após revisão e teste do owner.
