# AP-2026-11-10-1626 — Importação de bases grandes: criação sequencial não escala

- Status: capturado
- Escopo: projeto do cliente
- Task/SPEC: F1-T05 / SPEC 1.2 (importação CSV com prévia e relatório de erros)
- Sinal: importação em lote criando registros um por um (sequencial, await por linha) escala mal — 15.367 leads a ~6/s levou ~40 min; o usuário não vê o número subir rápido e interpreta como falha ("algo falhou" com o modal em 9600/15367). A criação sequencial também mantém a conexão HTTP aberta por dezenas de minutos (risco de timeout/aba fechada interrompendo o lote).
- Evidência: teste humano do owner com a base real do RD Station (15.387 contatos): prévia correta em segundos, mas a confirmação levou ~40 min; diagnóstico confirmou criação ativa a ~5,7/s (9919→10086 em 15s); 0 falhas de API no final.
- Regra reutilizável: importação em lote no frontend PocketBase deve (1) criar em lotes paralelos (Promise.all em blocos de ~20) ou usar batch API do PocketBase; (2) mostrar progresso com taxa estimada e tempo restante; (3) para bases >5k linhas, considerar importação server-side (hook com upload) em vez de cliente.
- Quando aplicar: F1-T18 (fixture baseline 10 leads — pequena, ok), qualquer importação futura de base grande (ex.: reimportação RD, integração Fase 2/3).
- Quando não aplicar: lotes pequenos (<200 linhas) — sequencial é simples e suficiente.
- Confiança: alta — medido na base real do cliente (15.367 registros, 0 falhas).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
