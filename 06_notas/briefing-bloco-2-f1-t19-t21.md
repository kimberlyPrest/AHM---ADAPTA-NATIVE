# Briefing — Bloco 2: F1-T19 + F1-T20 + F1-T21 (SPEC 1.7 — timeline de interações até proposta)

**Quem executa:** Claude Code (sessão AHM, via MCP Skip) — escreve e roda o QA do Skip.
**Quem revisa:** Bart (Maestro) — provas via API, validação de tela, docs e GitHub. NÃO edite fase.md/STATUS/changelog — isso é do Bart.
**Coordenação:** um escritor por vez. Ao terminar, o webhook avisa o Bart automaticamente.

## Contexto (o que JÁ existe — não recriar)

- Coleção `leads` (migration 0014-0017): nome, empresa, contato, produto_interesse, dor, urgencia, origem, status (novo|em qualificação|qualificado|pendente|fora do recorte|transbordo), qualificacao_resultado, qualificacao_pendencias, qualificacao_historico (json, montado no FRONTEND — hook JS não grava json), qualificado_em/por, observacoes.
- Tela `src/pages/Leads.tsx`: fila paginada server-side, busca server-side, modal Editar Lead, histórico de qualificação, motor local `src/lib/qualificacao.ts`.
- Versão corrente: 0.0.50. Base real: NÃO apagar Eduardo Zuin/Arkema, Aroldo Teles da Silva (Stellantis — preparado para testes), os 4 leads qualificados, a oportunidade real, os 2 transbordos reais.

## O que implementar

### F1-T19 — Timeline de interações no lead (CA-1-019)

1. **Migration 0021** (up/down completo): coleção `interacoes`:
   - `lead` (relation → leads, obrigatório)
   - `tipo` (select: Ligação | Reunião | E-mail | WhatsApp | Visita | Outro)
   - `data` (datetime, obrigatório — quando a interação ocorreu)
   - `responsavel` (relation → users)
   - `observacao` (text, opcional)
   - `proposta` (bool, default false — marca a interação que fechou a proposta, F1-T20)
   - `cancelada` (bool, default false) + `cancelada_em` (datetime) + `cancelada_motivo` (text) + `cancelada_por` (relation → users) — F1-T21
   - `criado_em` (datetime)
   - Índices: lead+data; regras de acesso: as 5 = `"@request.auth.id != ''"`.
2. **Hook** `pocketbase/hooks/on_interacoes_validate.js`:
   - `data` obrigatória (não pode ser futura além de 1 dia — tolerância de fuso).
   - `tipo` deve estar na lista.
   - Marcar `proposta = true` exige `observacao` preenchida (o que foi proposto).
   - Cancelamento (transição para `cancelada = true`) exige `cancelada_motivo` (F1-T21) — validação em model hook; auditoria `cancelada_em/por` em request hook (`onRecordUpdateRequest`, tem `e.auth`).
3. **Service** `src/services/interacoes.ts`: listar por lead (ordenado por data desc), criar, marcar proposta, cancelar. Paginação server-side se a lista crescer — para a Fase 1, listar por lead com `filter=lead='id'` e sort `-data` está OK (volume por lead é baixo).
4. **Tela**: no modal de detalhe do lead em `src/pages/Leads.tsx` (ou seção nova no modal Editar), acrescentar **Timeline de interações**:
   - Lista das interações do lead (tipo, data formatada pt-BR, responsável, observação, badge "Proposta" quando proposta=true, badge "Cancelada" quando cancelada=true).
   - Botão **Registrar interação**: tipo (select), data (input date), observação (textarea opcional).
   - Botão **Marcar proposta** em uma interação (exige observação; só uma interação pode ter proposta=true por lead — hook valida: se outra já tem proposta, bloquear com mensagem corporativa).
   - Botão **Cancelar interação** (exige motivo; preserva o registro — nunca apagar).
   - Toasts em pt-BR corporativo ("Interação registrada", "Proposta marcada", "Interação cancelada").

### F1-T20 — Marcação de proposta e contagem K2 (CA-1-020)

1. A marcação de proposta já sai da F1-T19 (campo `proposta` + validação única por lead).
2. **Contagem K2 (nº de interações até a proposta)**: no service `src/services/interacoes.ts`, função `contarAteProposta(leadId)`: nº de interações NÃO canceladas do lead até (inclusive) a que tem proposta=true. Se não há proposta, retornar `null` (não 0 — sem proposta ainda é diferente de proposta na 1ª interação).
3. Expor a contagem na timeline do modal do lead ("Proposta na interação X" ou "Sem proposta registrada — X interações até agora").

### F1-T21 — Cancelamento auditável (CA-1-021)

1. Cancelar interação: `cancelada=true` + motivo obrigatório + `cancelada_em/por` automáticos (request hook). O registro NUNCA é apagado.
2. Interação cancelada SAI da contagem K2 (função do F1-T20 já filtra `cancelada=false`) e mantém o histórico visível na timeline com badge "Cancelada" + motivo.
3. Prova de regressão: cancelar a interação com proposta → K2 volta a "sem proposta" (a contagem não conta canceladas).

## Regras obrigatórias (do CLAUDE.md — não negociável)

- **Linguagem**: todo texto de usuário em pt-BR corporativo — maiúscula inicial, pontuação completa, plural correto. NUNCA expor códigos internos (RN-x.x, SPEC, F1-Txx, K2) em strings de UI — a contagem K2 aparece como "Interações até a proposta", nunca "K2".
- **Migrations**: migration 0021 com down que desfaz; nunca editar migration aplicada.
- **Hooks**: validação em model hook; auditoria em request hook; todo caminho termina em `return e.next()` ou `throw new BadRequestError(...)`. JSVM não enxerga funções de topo — lógica DENTRO dos callbacks (lição da 0.0.47).
- **Sem getFullList** em coleções grandes; a timeline por lead pode usar filter direto.
- **QA**: `skip_project_apply_changes` verde (lint + build) antes de encerrar.
- **Base real**: não apagar registros reais. Provas com dados são do Bart (ele prepara o insumo do teste — AP-2026-11-12-1037).

## Definition of Done (o Bart valida isto)

- [ ] Migration 0021 aplicada (up/down OK, regras auth-only).
- [ ] Registrar interação funciona (tipo/data/responsável/observação).
- [ ] Marcar proposta exige observação; só uma proposta por lead; bloqueia segunda com mensagem corporativa.
- [ ] Contagem "Interações até a proposta" correta (null quando sem proposta; exclui canceladas).
- [ ] Cancelar exige motivo, preserva registro, grava cancelada_em/por, sai da contagem.
- [ ] Timeline visível no modal do lead com badges Proposta/Cancelada.
- [ ] QA do Skip verde.
- [ ] Zero códigos internos em strings de usuário; pt-BR correto.

Ao terminar: rode o apply_changes (o webhook avisa o Bart) e NÃO conclua a task — a conclusão é do Bart após revisão e teste do owner.
