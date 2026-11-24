# Re-prova B1 — bloqueio encerra sessão aberta (0.0.81/0.0.82) · Bart · 24/11/2026 18:40

**Contexto:** B1 da revisão do Bloco Segurança da equipe — bloquear não derrubava a sessão já aberta. CC anunciou correção na 0.0.81 via "filtro global que recusa qualquer requisição de usuário bloqueado". Re-prova executada contra o working tree 0.0.82 (HEAD 6b09746, não publicado).

## Resultado: **CORRIGIDO NA SUBSTÂNCIA — 12/12 provas decisivas PASS**

O usuário bloqueado **não lê nem escreve absolutamente nada**. Mas o mecanismo que age não é o filtro global anunciado: é o `refreshTokenKey()` do hook de bloqueio (seguranca_equipe.js), que mata o token na hora. As requisições com o token antigo passam a se comportar **exatamente como anônimas** — provado por equivalência byte a byte:

| Ação com token antigo | Resposta | Anônimo (sem token) |
|---|---|---|
| Listar leads | 200 com lista vazia | 200 com lista vazia |
| Ler detalhe da lead | 404 | 404 |
| Criar lead | 400 recusado | 400 recusado |
| Renovar sessão (auth-refresh) | 401 | 401 |
| Listar usuários | 200 com lista vazia | 200 com lista vazia |
| Novo login | 403 (authRule da 0039) | — |

**Equivalência provada**: token bloqueado ≡ anônimo em toda ação testada. Nada vaza, nada é gravado.

## Provas executadas (resumo)

**API (usuário temporário com lead própria):** criação → login OK → leitura OK → bloqueio com motivo → mesma requisição com mesmo token → lista vazia / detalhe 404 / escrita recusada / renovação 401 → novo login 403 → reativação → tudo volta a funcionar (login, leitura) → limpeza completa.

**Tela (preview, aba aberta como PROVA Vendedor):** bloqueio aplicado via API durante a sessão → navegação continua (menu visível) → tela de Leads mostra **"Nenhum lead cadastrado. Cadastre o primeiro lead ou importe sua base."** → formulário "Novo Lead" abre e aceita preencher → submissão **recusada pelo servidor** ("Erro ao criar lead / Failed to create record.") → nada gravado (conferido: 0 leads de prova na base).

## Riscos que encontrei além do pedido (protocolo de revisão)

1. **(BAIXA, UX) Bloqueado com aba aberta vê tela de "base vazia" convidando a cadastrar.** A leitura volta 200-vazia (comportamento anônimo), então a tela mostra o estado vazio padrão em vez de "seu acesso foi bloqueado". O usuário só descobre ao tentar escrever, com erro genérico. Correção futura: o frontend trata 401 em qualquer chamada → logout com mensagem "Seu acesso está bloqueado. Fale com o administrador." (mesma mensagem do hook). Não é bloqueante: nada vaza e nada é gravado. → **CORRIGIDO na 0.0.83 (ver revisao-debito-tela-0.0.83.md)**
2. **(BAIXA, mensagem) Erro de escrita do bloqueado é "Failed to create record."** — genérico em inglês, sem código estável. O padrão R8 (código + mensagem pt-BR) já previsto no contrato cobre isso no passo 2; registrar como caso de teste do passo 2.
3. **(INFORMAÇÃO) O "filtro global" (routerUse) não é o que protege de fato.** O código está correto na leitura, mas nas provas o efeito veio do `refreshTokenKey()` + regras de coleção (comportamento anônimo). Se o CC confirmar que o middleware não dispara nesse fluxo (possível limitação do JSVM: `routerUse` pode não ver `e.auth` de tokens de coleção), a defesa em profundidade atual (token morto + authRule + regras) já é suficiente — mas o relatório de correção do CC dizia "recusa QUALQUER requisição", o que não é literalmente o que acontece. Não é defeito de segurança; é precisão de relatório.

## Contas (autorização do owner, 24/11 18:22)

- **PROVA Vendedor (Bart)** criada (`prova.vendedor@ahmsolution.com`, papel vendedor) — senha forte gerada e guardada apenas na plataforma do Bart (nunca em arquivo/repo/mensagem). Conta ATIVA (é a conta de provas permanente; bloqueio só durante testes de bloqueio).
- **Rose Teste e Rose Teste 2** bloqueadas com motivo "Conta de teste" — executado pelo Bart sob autorização do owner (24/11 18:22). CORREÇÃO 24/11 18:48: o owner nunca tinha feito esse bloqueio (erro de redação na mensagem anterior dele); não há outro banco.
- Base intacta: 15.406 leads, 10 usuários, zero lixo de prova.

## Conclusão

B1 **corrigido** — o bloqueio encerra a sessão aberta na prática (token morto + login/renovação recusados + leitura/escrita bloqueadas). Bloco Segurança da equipe pronto para o "pode concluir" do owner junto com o teste humano. Achados de UX (1) e de mensagem (2) ficam registrados como débitos pequenos, sem bloquear.
