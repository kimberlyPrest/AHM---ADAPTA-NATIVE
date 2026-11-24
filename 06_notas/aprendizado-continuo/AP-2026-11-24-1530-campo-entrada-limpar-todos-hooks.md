# AP-2026-11-24-1530 — campo de entrada "nunca persiste" precisa ser limpo em TODOS os hooks

- **Data**: 2026-11-24T15:30:00-03:00 · **Task**: Bloco R1 (0.0.76–0.0.78) · **Capturado por**: Bart (revisão)
- **Situação**: o campo `etapa_ocorrido_em` (data real da troca de etapa) era limpo no hook de UPDATE mas não no de CREATE. Um POST com o campo preenchido o persistia, e o hook de update — que lê o valor "atual" do registro, não o payload — tratava o valor sujo como "data informada", recusando todo avanço de etapa futuro ("data anterior à etapa anterior").
- **Regra**: campo de ENTRADA (que o servidor consome e descarta) precisa ser limpo em TODOS os hooks que tocam a coleção — create E update. E, no update, o valor só conta como "informado" se DIFERIR do valor gravado (diferença payload × registro).
- **Prova que pega**: criar registro enviando o campo preenchido → conferir que ficou vazio; depois fazer a ação normal sem o campo → deve funcionar.
- **Generalização**: vale para `inicio_ocorrido_em` e `resultado_ocorrido_em` (0.0.78) — já nascem com a limpeza nos dois hooks.
