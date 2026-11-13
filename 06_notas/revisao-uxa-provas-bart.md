# Revisão do Bloco UX-A (0.0.68) — provas do Bart · 13/11/2026

**Protocolo aplicado (1ª revisão com o protocolo novo)**: pré-mortem com 3 riscos provados · regressão por papel nas telas · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

## Resultado: APROVADO COM RESSALVA — 24/24 provas de API verdes + tela OK, mas A3 (dado inventado no Dashboard) NÃO entrou nesta versão

## Provas do pedido (API) — 24/24 PASS

**(a) Vendedor vê especialistas (migration 0032)**
1. Vendedor lista users → 200 ✓
2. Nomes visíveis (Afonso Moreira, Beatriz Silva, Carlos Mendes, Mariana Costa, Rose Teste…) ✓
3. JSON do user SEM campos sensíveis (nenhum tokenKey/password; campos: avatar, emailVisibility, id, name, papel, verified…) ✓
4. Vendedor lista transbordos → 200 ✓
5. `expand=especialista` resolve o nome real para o vendedor ("Afonso Moreira") ✓

**(b)** Vendedor edita outro usuário → 404 (recusado) ✓
**(c)** Vendedor troca o próprio papel → 403 ✓

**(d) Regressão das 11 provas do Bloco 3** — todas verdes:
cadastro sem login 400 · vendedor cria PROVA 200 · vendedor apaga PROVA 403 · admin apaga PROVA 204 + auditoria "administrador" · admin apaga lead real 403 · renomear real→PROVA 400 · "Demolidora" aceita 200 · vendedor edita lead próprio 200 · auditoria/fechamentos 403 · produto vendedor 403/admin 200 · último admin protegido 400 · DEMO só admin (403/200/204) ✓

## Pré-mortem — 3 riscos provados

1. **View de nomes vazar dados sensíveis** → PROVADO: JSON do user para vendedor contém só campos públicos (name, papel, emailVisibility, avatar); nenhum tokenKey/password. ✓
2. **Login quebrar sem pré-preenchimento** → PROVADO NA TELA: campos vazios por padrão; login manual de vendedor e admin funciona; "Manter-me conectado" persiste sem pré-preencher senha. ✓
3. **`e.next()` duplo nos hooks after-success** → PROVADO: código revisado (`try { ... } finally { e.next() }` nos 2 hooks, caminho de erro não chama e.next() duas vezes); PATCH/DELETE de lead executados sem erro; logs sem NOVO warning de e.next() após as operações (o último warning é de 12/11 12:53, antes da correção). ✓

## Varredura de segurança

- Login sem credencial: `grep` no Login.tsx → 0 ocorrências de e-mail/senha hardcoded; `useState('')` nos dois campos ✓
- "Solicitar acesso" sem contatos inventados: modal diz "Fale com o seu gestor…" — sem TI/ramal/suporte@ inventados ✓
- index.html: `lang="pt-BR"`, título "AHM Solution — Portal Comercial", `robots noindex, nofollow` ✓
- users sem login → lista vazia; cadastro sem login → recusado ✓
- Regras de auditoria/fechamentos intactas (só admin) ✓

## Regressão por papel NA TELA (todas as telas)

**Vendedor (Julio)**: login OK → Dashboard, Tickets, Atendimentos, Produtos, Leads, Transbordos, Oportunidades, Métricas carregam todas ✓ · Transbordos mostra **"Especialista: Afonso Moreira"** (nome real — fim do "Sem especialista atribuído" falso) ✓ · header mostra "Julio / Vendedor" (papel real, sem "Online" fixo) ✓ · rodapé sem "v1.0.0" ✓ · menu SEM Equipe (correto) ✓

**Admin (Afonso)**: login OK → menu COM Equipe ✓ · header "Administrador" ✓

## Riscos que encontrei além do pedido

1. **A3 NÃO ENTROU nesta versão (alta)**: o Dashboard ainda exibe a frase inventada **"Alta eficiência da equipe comercial"** (sob o KPI "Resolvidos Hoje" = 0) e o **"SLA corporativo meta: < 30 min"** fixo — provado na tela como admin. Era item do briefing do UX-A; não bloqueia o que foi entregue (A1/A2/A5/A6 todos resolvidos), mas o A3 fica pendente para a próxima rodada (ou entra no UX-B). Decisão do owner pendente: SLA vira configuração ou sai da tela.
2. **Pendência de segurança do próprio owner (alta, ação do Afonso)**: a senha exposta nas versões anteriores (`Skip@Pass`) e a atual (`AHM@Atende2026`) circularam em código/repositório — trocar a senha do admin. (Registrada pelo CC; reforço aqui.)
3. **Base de leads com resíduos de prova (baixa)**: 5 leads "PROVA B3 Ven*" de rodadas anteriores da revisão do Bloco 3 foram encontrados na listagem da tela e apagados na limpeza desta revisão. Base agora: 15.394 (15.370 reais + insumos PROVA do teste do Bloco 2 + resíduos do owner). Sem risco real — eram todos PROVA apagáveis — mas registro que a limpeza anterior deixou passar 5.

## Limpeza

5 leads PROVA B3 Ven* apagados; lead "Demolidora Comércio UXA" arquivado com motivo; lead DEMO da prova criado e apagado; lead PROVA UXA PM3 (pré-mortem 3) criado e apagado. Lead real Aroldo íntegro.

## Conclusão

**Bloco UX-A APROVADO** (0.0.68): login limpo, nomes de colegas visíveis ao vendedor com a privacidade correta, hooks de realtime corrigidos, regressão completa verde. Ressalva: A3 (dado inventado no Dashboard) não entrou — cobrar na próxima rodada.
