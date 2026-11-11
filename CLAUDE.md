# CLAUDE.md — Projeto Atendimento AHM (Skip Cloud)

Instruções para o Claude Code trabalhando neste projeto. O projeto vive no Skip Cloud
(projeto "Atendimento AHM", id 59655) sincronizado com este repo GitHub.

## Papéis de trabalho (dois escritores coordenados)

- **Claude Code**: implementa (migrations, hooks, telas) direto no Skip via MCP (skip_file_write/patch + apply_changes) e/ou neste repo, e avisa o Maestro pelo webhook.
- **Bart (Maestro)**: revisa (provas via API + validação de tela no preview), atualiza docs do repo (fase.md, STATUS.md, changelog, .adapta-cliente/estado-atual.md) e conduz os portões com o owner.
- **Regra de ouro**: UM escritor por vez nos mesmos arquivos. Antes de editar, conferir o estado no git e no Skip working tree. Nunca editar em paralelo sem coordenação pelo chat.

## Ritmo do projeto (protocolo Adapta — obrigatório)

1. Uma task por vez, sempre a próxima elegível da `04_fase-atual/fase.md`.
2. Análise (baseline via API: o que existe/não existe) → **parar no portão**: pedir autorização explícita do owner ("Pode implementar?").
3. Implementar → provas automáticas via API (scripts em `scripts/` do Maestro) + prova de tela no preview → **parar no portão de teste humano**.
4. Só concluir após aprovação explícita do owner ("pode concluir"). Então: atualizar fase.md/STATUS/changelog/estado-atual.md + capturar aprendizado contínuo + push GitHub.
5. "Faça tudo" ou o pedido inicial NÃO autoriza implementação — o gate é uma mensagem nova do owner autorizando a task específica.

## Convenções do Skip (obrigatórias)

- **Migrations**: numeração ordinal de 4 dígitos (próxima = conferir `pocketbase/migrations/` — atual: 0019). Migration nova para CADA mudança de schema (nunca editar migration aplicada). Sempre `migrate(up, down)` com down que desfaz.
- **Coleções**: todas as 5 regras de acesso (`listRule/viewRule/createRule/updateRule/deleteRule`) = `"@request.auth.id != ''"` (padrão da Fase 1). Campos relation com `collectionId` resolvido via `app.findCollectionByNameOrId(...)`.
- **Hooks PocketBase** (`pocketbase/hooks/*.js`):
  - Validação de dados → `onRecordCreate`/`onRecordUpdate` (model hooks, SEM `e.auth`).
  - Auditoria "quem fez" → `onRecordCreateRequest`/`onRecordUpdateRequest` (request hooks, COM `e.auth`).
  - Efeito em registro vinculado após commit → `onRecordAfterUpdateSuccess`.
  - Todo caminho termina em `return e.next()` ou `throw new BadRequestError(...)`; hook com filtro de coleção no último argumento (`}, 'leads')`).
  - Hook JS não grava campo json com array (invalid_json) nem string (charcodes) — histórico json é montado no FRONTEND.
- **Telas React** (`src/pages/`): página nova = arquivo em src/pages + rota em `src/App.tsx` + navItems em `src/components/Layout.tsx` + título no `getPageTitle`. Listagem SEMPRE paginada server-side (getList, 20/página) — NUNCA getFullList em coleção grande (leads tem 15 mil).
- **Filtros/busca**: a UI constrói a expressão completa (`status='valor'`) ANTES de chamar o service; service recebe expressão, nunca valor cru.
- **QA**: `skip_project_apply_changes` roda lint+build+commit; build quebrado = corrigir ANTES de prosseguir. Após apply, revalidar por API e na tela do preview.

## Aprendizados capturados (NÃO repetir — ver 06_notas/aprendizado-continuo/)

1. **Acentos em matching pt-BR** (AP-2026-11-10-2149): busca por palavra-chave em texto de usuário (dor, motivo) deve normalizar acentos nos DOIS lados (`normalize('NFD').replace(/[\u0300-\u036f]/g, '')`). Provar com o texto exato como o usuário digitaria (com ç, ã).
2. **Expressão de filtro** (AP-2026-11-11-0545): UI monta `campo='valor'` completo antes do service. Prova de tela com clique real pega o que a prova de API não pega — rodar as duas.
3. **Efeito visível** (AP-2026-11-11-0600): quando uma ação altera registro vinculado (ex.: resolução de transbordo → lead), a tela onde o usuário está DEVE mostrar o estado resultante do registro vinculado. Prova de API confirma o dado; prova de UX confirma a percepção — o cliente testa a segunda.
4. **Patch ambíguo**: se o SEARCH/REPLACE falhar ou tocar texto com marcadores (`=======`), REESCREVER o arquivo inteiro com skip_file_write em vez de insistir no patch.
5. **SDK PocketBase omite campos undefined** no update — para LIMPAR um valor, enviar string vazia explicitamente.
6. **DELETE responde 204 sem corpo** — parser JSON de scripts de prova deve tratar resposta vazia.

## Estado e documentos

- Estado corrente da task: `.adapta-cliente/estado-atual.md` (etapa, autorizações, provas).
- Fila de tasks e critérios: `04_fase-atual/fase.md`. SPECs: `04_fase-atual/specs/`.
- Histórico: `changelog.md`. Progresso: `STATUS.md`. Aprendizados: `06_notas/aprendizado-continuo/`.
- Base real (não apagar): lead Eduardo Zuin/Arkema (uso legítimo da fila de transbordo), 15 mil leads importados (13 mil rascunhos), 2 usuários de teste invisíveis.
- Registros de prova SEMPRE limpos após a verificação (leads/produtos/oportunidades/transbordos com prefixo "PROVA").

## Push GitHub

- Push local falha (sandbox sem credencial) — usar MCP GitHub `push_files` (owner kimberlyPrest, repo AHM---ADAPTA-NATIVE, branch main) com o CONTEÚDO COMPLETO de cada arquivo (push parcial TRUNCA o arquivo no repo), depois `git pull --rebase` no clone local.
- Se o rebase travar com commit duplicado: `git rebase --abort` → `git reset --hard origin/main` (o GitHub já tem a versão correta).
