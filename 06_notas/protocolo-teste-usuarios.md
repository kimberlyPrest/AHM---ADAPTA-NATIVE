# Protocolo de teste com usuários — CRM AHM (Adapta Native)

**Versão**: 1.0 · **Data**: 13/11/2026 · **Dono do formato**: Bart · **Decisão do owner**: o time de vendas testa; o Afonso gere e idealiza; o Bart desenha o teste, valida as respostas e identifica gaps.

## Princípios (para o testador se sentir confortável)

1. **Não existe resposta errada.** O que está sendo testado é o sistema, não a pessoa. Se algo confundiu, isso é um achado valioso — é exatamente o que queremos coletar.
2. **Teste no seu ritmo, sozinho.** Ninguém está olhando por cima do ombro. Sem cronômetro cobrado.
3. **Erro não é problema.** Se travar, errar ou não achar um botão: anote. Isso vira melhoria, não reprovação.
4. **Tempo curto.** Cada sessão tem 15–20 minutos, dividida em cenas do seu dia a dia real.
5. **Use dados de teste.** Os registros com "PROVA" no nome são descartáveis — pode mexer, fechar, reabrir sem medo. NUNCA mexa em lead real (nome sem "PROVA").

## Formato da sessão

Cada bloco novo do sistema gera um **roteiro de cenas** (o Bart prepara). Cada cena tem:

- **Situação**: um caso real do trabalho do vendedor, contado como história ("Um lead chegou do site e você precisa registrá-lo…").
- **O que fazer**: passos soltos — o testador tenta sozinho antes de olhar o passo a passo.
- **Ficha de resposta** (3 perguntas fixas + 1 aberta):
  1. Conseguiu concluir? → **Sim / Sim, com dificuldade / Não**
  2. Quanto tempo levou? → **< 1 min / 1–3 min / mais de 3 min / desisti**
  3. Nota de conforto (1–5): 1 = "me perdi completamente" · 5 = "farei isso no dia a dia sem pensar"
  4. Aberta: "O que esperava que acontecesse e não aconteceu?" (pode responder "nada")

## Critérios de validação (usados pelo Bart, não pelo testador)

| Sinal | Verde | Amarelo | Vermelho |
|---|---|---|---|
| Conclusão | ≥ 80% das cenas concluídas sem ajuda | 60–79% | < 60% |
| Conforto médio | ≥ 4,0 | 3,0–3,9 | < 3,0 |
| Ajuda necessária | ≤ 1 cena com ajuda | 2–3 cenas | > 3 cenas |
| Bloqueios/erros | nenhum comportamento inesperado | 1–2, contornáveis | qualquer perda de dado ou bloqueio sem saída |

**Gap** = qualquer resposta "Não", conforto ≤ 2, ou expectativa não atendida na pergunta aberta. Cada gap vira item no relatório ao CC (com o ponto exato) ou emenda, conforme a gravidade.

## Fluxo por bloco

1. **Bart** prepara: roteiro de cenas + insumos PROVA prontos na base + ficha de respostas (papel ou formulário digital simples).
2. **Owner** indica 1–2 vendedores testadores (rotatividade ao longo das fases — todos passam pelo sistema).
3. **Testador** faz a sessão (15–20 min) e responde as fichas.
4. **Bart** tabula, aplica os critérios, escreve o relatório de gaps e manda ao CC o ponto exato do que corrigir — antes de qualquer limpeza das provas.
5. **Owner** recebe resumo de 1 parágrafo: o que passou, o que virou gap, o que foi decidido.
6. Só depois do resultado verde (ou dos ajustes corrigidos e re-testados) o bloco é concluído.

## O que NÃO se pede ao testador

- Nada técnico (nomes de campo, coleções, códigos, migrations).
- Nada de comparar com outros sistemas.
- Nada de "está bom assim?" genérico — sempre amarrado a uma cena concreta.

## Sessão atual

- **Bloco 2 da Fase 2** → roteiro de cenas em `06_notas/roteiro-teste-f2-bloco2-usuarios.md` (adaptação do roteiro técnico para linguagem de vendedor).
