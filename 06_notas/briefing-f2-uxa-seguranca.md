# Briefing — Bloco UX-A: Segurança e integridade de dados na tela (F2-T12)

**Data**: 13/11/2026 · **Autorização**: pendente do owner · **Prioridade**: URGENTE — Julio e Rose estão usando a plataforma agora.

**Contexto**: feedback do owner (13/11) sobre o papel de revisão — o Bart agora aplica protocolo obrigatório (pré-mortem, regressão por papel, varredura de segurança, dado inventado = defeito, logs, riscos fora do escopo, evidência antes de bloquear). Este briefing já nasce desse protocolo: os itens A5 e A6 abaixo são achados do Bart nos logs e no código, além do que o CC já havia sinalizado.

## Problemas a corrigir

### A1. Login expõe credenciais do administrador (BLOQUEANTE de segurança)
- `src/pages/Login.tsx`: `useState('afonso.moreira@ahmsolution.com')` e `useState('Skip@Pass')` — e-mail e senha do admin pré-preenchidos no formulário, visíveis para qualquer pessoa que abra o preview.
- Nota: a senha do admin já foi trocada (AHM@Atende2026), então `Skip@Pass` não entra mais — mas continua expondo o e-mail do administrador e a senha antiga do seed.
- **Correção**: campos de login vazios por padrão, sem nenhum valor pré-preenchido, sem "dica" de credencial. Remover também qualquer autocomplete que sugira a senha salva.

### A2. Vendedor não vê o nome dos colegas (falha do Bloco 3 que as provas não pegaram)
- A rule de `users` (`list: id = @request.auth.id || papel = 'admin'`) impede o vendedor de listar colegas. Consequência em cadeia:
  - **Transbordos**: `usersService.list()` falha para vendedor (catch silencioso → lista vazia) e o `expand?.especialista?.name` não resolve → transbordo atribuído a outra pessoa aparece como **"Sem especialista atribuído"** — informação FALSA na tela.
  - **Oportunidades/leads**: atribuição de responsável fica sem nome para o vendedor.
- **Correção sugerida pelo CC**: migration criando uma view/regra que permita a usuários autenticados ler apenas `name` (e opcionalmente `email`) dos usuários ativos — nunca senha, token ou papel de quem não pode ver. Alternativa: endpoint `/backend/v1/usuarios/nomes` (auth-only) devolvendo id+nome.
- **Critério**: vendedor vê "Especialista: [nome real]" no transbordo; nunca mais "Sem especialista atribuído" para transbordo atribuído.

### A3. Dado inventado na tela inicial (Dashboard)
- `src/pages/Dashboard.tsx`: frase fixa **"Alta eficiência da equipe comercial"** sob o KPI "Resolvidos Hoje" — aparece sempre, mesmo com 0 resolvidos. Informação que não vem do banco = dado inventado.
- **Correção**: remover a frase ou substituí-la por texto neutro/condicional (ex.: mostrar tendência real vs. ontem, calculada dos dados).
- Também no Dashboard: **"SLA corporativo meta: < 30 min"** — meta fixa no código, não configurada em lugar nenhum. Ou vira configuração editável pelo admin, ou sai da tela. Decidir com o owner.

### A4. Tela Equipe — conferir que não restam dados inventados
- A versão atual do `Equipe.tsx` já está limpa (sem avaliações nem status "online" inventados — corrigido no Bloco 3). Registrar como verificação de regressão, não como trabalho novo.

### A5. (Achado do Bart nos logs) Hooks after-success terminam sem `e.next()`
- Logs do Skip (11–12/11, recorrente): `served hooks can finish without calling e.next(): on_leads_oportunidade (onRecordAfterUpdateSuccess lines 12/38/40/46/59/61/67), on_transbordo_validate (lines 57/59/77)` — após a escrita do registro, o **realtime update para os clientes inscritos e os demais handlers after-success nunca rodam**.
- Consequência em produção: telas abertas podem não atualizar em tempo real após mudanças de outros usuários; handlers subsequentes são pulados.
- **Correção**: terminar todo caminho que prossegue com `e.next()` (padrão sugerido pela plataforma: `try { ... } finally { e.next() }`) nos dois hooks.

### A6. (Achado do Bart — varredura de regras) Regras de auditoria corretas, mas sem hook de create
- `exclusoes_auditoria` e `oportunidade_fechamentos`: rules `@request.auth.papel = 'admin'` em todas as operações — correto. A escrita só-server-side depende de nenhum cliente conseguir `create` — confirmado que a rule bloqueia. Sem ação; registrado como verificação positiva.

## Pré-mortem do Bart (3 jeitos de este bloco quebrar em produção)

1. **A view de nomes de usuários vazar mais dados que o pretendido** (ex.: papel, e-mail de todos para qualquer um autenticado) → prova: vendedor consulta a view e confere que só vê nome (e o que for acordado); tentar expandir para coleção sensível → bloqueado.
2. **Remover o pré-preenchimento do login quebrar o fluxo de "lembrar-me"** ou o login do owner → prova: login manual do admin e de vendedor funciona; "lembrar-me" persiste sessão sem pré-preencher senha.
3. **O `finally { e.next() }` nos hooks after-success disparar e.next() duas vezes** (no caminho de erro E no de sucesso) → prova: atualizar lead/transbordo e conferir que o realtime chega na tela aberta e que NÃO há warning novo nos logs.

## Critérios de aceite (provas do Bart)

1. Login sem credenciais pré-preenchidas; código sem nenhuma senha hardcoded (varredura `grep` nos src/).
2. Vendedor: vê nome do especialista nos transbordos atribuídos; vê colegas na atribuição de responsável; NÃO vê papéis/dados sensíveis de outros usuários além do acordado.
3. Dashboard sem frase inventada; SLA meta decidido com o owner (configurável ou removido).
4. Logs sem novos warnings de `e.next()` após operações de lead/transbordo; realtime funciona em duas sessões abertas.
5. Regressão por papel: TODAS as telas (Dashboard, Leads, Oportunidades, Transbordos, Métricas, Equipe, Produtos) como admin e como vendedor — nada piorou.
6. Varredura de segurança: nenhum endpoint aberto sem login; nenhuma senha/credencial no código do frontend.

## Fora do escopo, mas registrado (não bloqueia)

- Modal de oportunidade em 512px pode ser a causa das 4 tentativas seguidas de trocar etapa sem fechamento (sinal de UX do teste) — entra no Bloco UX-B (telas), com confirmação de Julio/Rose.
- Tela inicial "mesa do vendedor" — Bloco UX-C.
- Emenda 1.3a (IA) aguarda ratificação + 10 casos — próximo bloco grande depois dos UX.
