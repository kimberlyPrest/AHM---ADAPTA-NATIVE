# AP-2026-11-12-2135 — Meta de desempenho de importação: latência do servidor domina

**Contexto**: revisão do Bloco 5 (importação incremental + paralelismo no cliente).

**O que aconteceu**: 1.000 criações paralelas (10 simultâneas) levaram 57s (57 ms/lead) — o paralelismo no cliente funciona (sequencial era ~31 ms/lead → 4 min p/ 7,5 mil), mas a meta do briefing (<1 min p/ 7.500) não é atingível no cliente: a latência por criação no servidor domina. Medição com keep-alive (10 conexões TLS persistentes, mais fiel ao fetch do navegador) piorou para 213s — o gargalo não é o handshake, é o tempo de processamento por criação (hooks de validação por registro).

**Sinal reutilizável**: paralelizar o cliente tem teto = latência servidor × (N/concorrência). Para 10× mais throughput, o caminho é endpoint de criação em lote no servidor (1 requisição para N registros) — registrado como sugestão pós-Fase 1. Medir com e sem keep-alive antes de atribuir o gargalo à rede.
