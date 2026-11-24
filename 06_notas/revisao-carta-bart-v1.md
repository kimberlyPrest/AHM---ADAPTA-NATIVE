# Revisão da CARTA-BART.md v1.1 — revisão única do Bart · 24/11/2026

**Estado conferido em 24/11 12:35: versão 0.0.74, última migration 0035, working tree limpo (repo e Skip), último commit f703a61.**
**Base da revisão**: texto da v1.1 colado pelo owner no chat (o arquivo ainda não está no repo nem no Skip working tree — ver pendência no fim).

## Resultado: APROVADA COM 4 AJUSTES (3 menores, 1 de conteúdo) — nada estrutural

A carta está boa: separa o que vale para tudo (Parte I) do mandato dos sistemas (Parte II), fecha os dois problemas diagnosticados (instrução aberta + estado espalhado), e os formatos fixos são verificáveis. Os ajustes abaixo não mudam estrutura — são correções de precisão.

## Checklist contra o que foi acordado

| Acordo (conversa de 24/11) | Na carta? |
|---|---|
| Carta mora em CARTA-BART.md no repo; CLAUDE.md só aponta | ✅ seção 5 (e CLAUDE.md "sem estado, sem números") |
| Instruções da plataforma = 2–3 linhas apontando para a carta + STATUS | ✅ Parte II, leitura obrigatória |
| 4 regras adotadas (conferir antes de escrever; 1 decisão = 1 mensagem; pergunta fechada A/B; travou escala) | ✅ seção 4, itens 1–4 |
| Acréscimo do owner: convenção não cita número que envelhece | ✅ seção 4, item 5 |
| Divisão de portões (doc reversível = Bart faz e avisa; código/Skip/dados/negócio = owner) | ✅ seção 3 |
| Estado único STATUS.md; fase.md = fila; changelog só na conclusão; estado-atual aposentado | ✅ seção 5 |
| R1–R10 como critério de aceite (emenda 1.4a) | ✅ seção 7, com referência ao documento canônico |
| Fases A/B/C com gatilhos | ✅ seção 9 |
| Contexto permanente (pessoas, base real, feedback da Rose) | ✅ seção 10 |

## Ajustes pedidos (para a v1.2 — o CC aplica, eu não reescrevo)

1. **(conteúdo) Seção 3 — "Rodar provas de leitura por API" precisa de exceção explícita para o ciclo de provas.** Como está, a coluna "pode sozinho" proíbe o que a seção 8 me manda fazer: minhas provas de revisão criam e **gravam** registros PROVA (lead, produto, oportunidade, janela, regra) e depois os limpam. Hoje isso é autorizado por task no briefing. Sugestão de linha: "Criar e limpar registros com prefixo PROVA **dentro do ciclo de revisão de uma task autorizada** (o briefing lista o que a prova toca); fora disso, precisa de autorização". Sem isso, a carta se contradiz na primeira revisão.
2. **(menor) Seção 10 — feedback da Rose: os 2 arquivos citados ainda não existem no repo** (`06_notas/analise-feedback-rose-2026-11-24.md` e `06_notas/proposta-padrao-operavel-por-agentes.md` — conferido agora, working tree limpo). Ou o CC os publica junto com a v1.2, ou a carta aponta "a publicar pelo CC". Referência a arquivo inexistente é o mesmo defeito de "afirmar sem conferir a fonte" que a carta proíbe.
3. **(menor) Seção 5 — quem atualiza o quê**: a coluna diz "Bart" para STATUS/fase/changelog/CLAUDE.md, mas a seção 2 (Não faz) diz "não altera o Skip" — e esses arquivos vivem no repo E no working tree do Skip. Vale explicitar: "Bart atualiza no **repo GitHub** (push_files); o sync para o working tree do Skip é feito pelo CC ou pelo sync do GitHub". Evita ambiguidade sobre quem toca o Skip.
4. **(menor) Parte I, regra 1 — exceção já existe e está correta, falta o registro**: "a não ser que ele tenha autorizado expressamente aquela rotina" — hoje as rotinas autorizadas a publicar sozinhas são: relatório diário de acidentes (e-mail 08:00) e conteúdo LinkedIn (rascunho por e-mail; publicação só com sinal verde). Vale listar as rotinas autorizadas em uma linha, para a exceção não ficar aberta à interpretação.

## O que está melhor que a v0 (registro para o owner)

- A Parte I resolve o risco de a carta "sequestrar" as outras frentes (notícias, LinkedIn) — elas seguem como estão.
- "Se não conseguiu conferir, escreva NÃO CONFERIDO" é melhor que a minha versão — fecha o caso CLAUDE.md/0019.
- A regra-mãe do R1–R10 (agente registra fatos; decisão sugere, humano confirma) está citada com a referência canônica.
- Formato de mensagem ao owner (máx. 10 linhas) é verificável e resolve o problema das mensagens longas.

## Pendência operacional (não bloqueia a aprovação)

O arquivo `CARTA-BART.md` **ainda não está no repo nem no Skip working tree** (conferido agora). Para a carta valer como documento vivo, o CC publica a v1.2 (com os 4 ajustes) no repo; aí eu confirmo em 1 linha e o owner aprova final.

## Próximo passo

CC aplica os 4 ajustes → publica CARTA-BART.md v1.2 + os 2 arquivos citados no repo → eu confirmo a v1.2 em 1 linha (sem nova revisão completa) → owner aprova.
