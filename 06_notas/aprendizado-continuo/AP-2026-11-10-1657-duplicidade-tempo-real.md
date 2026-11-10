# AP-2026-11-10-1657 — Consulta de duplicidade em base grande: filtro exato + debounce

- Status: capturado
- Escopo: projeto do cliente
- Task/SPEC: F1-T06 / SPEC 1.2 (sinalização de duplicidade local, RN-1.5)
- Sinal: com 15 mil leads na base, sinalizar duplicidade em tempo real no formulário exige consulta por tecla — sem debounce e com filtro amplo, cada digitação varre a base inteira e a UI fica instável. O filtro precisa ser EXATO (contato + empresa, não busca parcial ampla) e case-insensitive implícito no PocketBase.
- Evidência: implementação com debounce 600ms + filtro `contato~X && empresa~Y` (com fallback do prefixo do e-mail) respondeu rápido na base de 15.370 registros; banner apareceu corretamente para duplicado real (2 leads do Aroldo) e não apareceu para contato novo (0 falsos positivos).
- Regra reutilizável: validação de duplicidade em tempo real no frontend PocketBase = debounce (500–700ms) + filtro composto exato + estado de "verificando" na UI + nunca bloquear (RN-1.5 pede aviso com confirmação, não índice único).
- Quando aplicar: F1-T13 (oportunidade a partir de lead qualificado — duplicidade de oportunidade), qualquer formulário de criação sobre coleção grande.
- Quando não aplicar: coleções pequenas (<500 registros) — consulta direta sem debounce é suficiente.
- Confiança: alta — testado na base real de 15.370 leads com duplicado real e falso positivo verificado.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
