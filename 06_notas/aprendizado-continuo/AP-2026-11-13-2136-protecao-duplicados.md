# AP-2026-11-13-2136 — Proteção contra duplicados precisa ser determinística e cobrir chaves alternativas

**Contexto:** Bloco 5 (importação incremental). O incidente de 12/11 (7.548 duplicados) mostrou que avisar não basta; a 0.0.58 passou a pular duplicados, mas as provas encontraram 2 falhas na proteção.

**Aprendizados:**

1. **Chave de duplicidade deve cobrir registros com campos ausentes de forma SIMÉTRICA.** A chave secundária (nome+empresa) só existia para a linha do arquivo; o registro da base gerava só a chave principal (contato+empresa). Resultado: linha sem e-mail nunca reconhecia lead cadastrado com e-mail. Correção: `chavesDoRegistroBase` — todo registro gera a principal E a secundária; a linha aceita registra as duas.
2. **Leitura paginada sem conferência de cobertura não é verificação — é sorte.** `items.length < pageSize → break` encerra silenciosamente com página truncada/erro transitório, e a verificação "passa" com base parcial (prévia classificou 15.370 existentes como novos). Correção: `lerTodasPaginas` — total contado na 1ª página, página parcial intermediária ou soma abaixo do total = erro; `comTentativas` (4×, espera crescente) por página; `requestKey: null` para o SDK não cancelar consultas concorrentes; `sort: created,id` para inserções não deslocarem páginas já lidas.
3. **Falha na verificação deve BLOQUEAR, não avisar.** O mecanismo de bloqueio (banner + botão desabilitado) já existia; o defeito era a verificação "passar" indevidamente. Prova de robustez: interceptar fetch e falhar a página 5 → bloqueio; prévia normal na sequência → recuperação.
4. **Prova de tela com arquivo grande:** browser do sandbox não alcança localhost — injetar CSV via localStorage em pedaços (~80KB) + `DataTransfer` no `input[type=file]`.

**Regra para o futuro:** qualquer comparação "arquivo × base" em volume precisa de (a) chaves simétricas, (b) conferência de cobertura, (c) bloqueio em caso de dúvida.
