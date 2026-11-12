# Briefing — Bloco 5: Importação incremental (somente novos) + desempenho

**Data:** 12/11/2026 · **Autor:** Bart (revisão/portões) · **Decisão do owner:** Afonso Henrique

## Contexto

Incidente de 12/11: a reimportação da base RD criou 7.548 leads duplicados. A prévia sinalizou as duplicidades corretamente, mas a execução gravou tudo — a duplicação ficou a cargo da decisão humana em cima de 15 mil linhas, o que não é operável.

**Decisão do owner (12/11):** a importação deve trazer SOMENTE os leads novos. Duplicados nunca são gravados. A base já está lá; a importação é para leads novos.

Pendência técnica correlata (AP-2026-11-10-1626): a criação é sequencial (~31 ms/lead → ~4 min para 7,5 mil). Aproveitar o mesmo bloco para paralelizar.

## Escopo

### 1. Importar somente os leads novos (comportamento padrão, sem opção de importar duplicados)

- Na execução, toda linha classificada como duplicada na prévia é **pulada automaticamente** (nenhuma requisição de criação para ela).
- Duplicado interno do arquivo (mesma linha repetida no CSV): a primeira ocorrência grava, as seguintes são ignoradas.
- A prévia continua exibindo as duplicidades (regra RN-1.5) — o que muda é a execução, não o aviso.
- O texto da prévia deve alinhar a expectativa: algo como "A importação gravará somente os leads novos. Os duplicados serão ignorados." (linguagem corporativa, sem códigos internos).
- Atualização de lead existente continua sendo pelo Editar Lead. **Fora do escopo:** atualização em massa via CSV (sobrescrever telefone etc.) — só se o owner pedir depois.

### 2. Desempenho: criação paralela em lote

- Criar os leads novos em lotes paralelos (sugestão: 10 simultâneos; medir e ajustar se o servidor recusar).
- Meta: 7,5 mil leads em menos de 1 minuto (hoje: ~4 min).
- A barra de progresso continua funcionando (contador incremental por lote concluído).
- Item que falhar: 1 nova tentativa; falha definitiva → registrar e seguir. O relatório final mostra quantos não entraram e por quê.

### 3. Relatório do resultado

- Ao concluir: "X novos leads importados. Y já existentes foram ignorados." (+ "Z não importados por erro" se houver).
- Plural correto: "1 novo lead importado. 2 já existentes foram ignorados."

## Critérios de aceite (provas que o Bart executará)

1. Reimportar a base RD completa (15.370 linhas) → **0 duplicados criados**; base permanece 15.370; relatório mostra os ignorados.
2. CSV de prova com 3 linhas: 1 nova, 1 duplicada da base, 1 duplicada interna do arquivo → 1 gravado, 2 ignorados, mensagem correta.
3. Tempo de importação de ~1.000 leads medido (meta: < 1 min).
4. Linguagem: plural e maiúscula corretos em todas as mensagens novas; nenhum código interno exposto.
5. Regressão: importação normal de CSV novo continua funcionando (telefones, observações, prévia, campo E-mail do contato).

## Observações técnicas

- Mudança é no cliente (pular antes de criar + paralelismo + relatório). Migration não deve ser necessária; se julgar necessário algo persistente, justificar como desvio.
- A consulta de duplicidades do Bloco 4 (lotes de 30 / varredura paginada) permanece como está — é a fonte da classificação NOVO/DUPLICADO.
- Cuidado com o hook de leads: criação paralela dispara vários hooks simultâneos; confirmar que não há condição de corrida na verificação de duplicidade do servidor (se houver, documentar).

## Entrega

- Versão publicada no Skip + resumo dos desvios justificados (padrão dos blocos anteriores).
- Testes automáticos novos: duplicado da base ignorado, duplicado interno ignorado, contagem do relatório.
- Ao terminar, avisar o Bart para as provas. Teste humano consolidado depois.
