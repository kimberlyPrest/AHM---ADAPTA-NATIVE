# AP-2026-11-12-1310 — Refs de navegador desatualizadas podem acionar ação destrutiva

**Contexto**: revisão do Bloco 3 (F1-T16..T18) — prova de tela do painel Métricas.

**O que aconteceu**: após a página re-renderizar (filtro mudou), um clique usando ref capturada antes do re-render caiu no botão "Remover demonstração" em vez do botão pretendido; o diálogo de confirmação acabou aceito e a massa DEMO foi removida sem intenção (sem dano: só dados de demonstração, ciclo refeito depois).

**Sinal reutilizável**: refs de snapshot ficam DESATUALIZADAS após qualquer re-render — recapturar o snapshot antes de TODO clique em tela dinâmica; diálogos de confirmação de destruição não devem ser aceitos por automação sem leitura prévia do contexto (se um diálogo aparecer inesperado, dismiss e recapturar).

**Correlato**: prova de tela com clique real continua pegando o que a prova de API não pega (AP-2026-11-11-0545) — mas exige higiene de refs.
