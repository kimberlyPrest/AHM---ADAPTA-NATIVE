# AP-2026-11-10-2106 — Lista de coleção grande: paginação server-side obrigatória + revalidação completa após refatoração

- Status: capturado
- Escopo: projeto do cliente
- Task/SPEC: F1-T09 / SPEC 1.3 (+ correção de desempenho da tela /leads)
- Sinal: dois problemas encadeados na migração da lista de leads para paginação server-side — (1) padrão "carregar tudo" (getFullList) não escala: 15.370 leads = 154 requisições, ~7s, ~10MB em memória, refazendo após cada ação; a tela travava na digitação. (2) a refatoração quebrou a tela: variável `filtered` removida do estado mas referenciada no empty state → erro JS "filtered is not defined" → tela em branco, reportada pelo owner na recarga.
- Evidência: antes: 154 req/~7s/~10MB; depois: 1 req de 20 itens = 0,08-0,10s, contadores agregados 0,02-0,04s, busca server-side 0,07s. Tela em branco reproduzida no navegador do Bart (root vazio, onerror capturado), corrigida em 0.0.28 (leads.length no empty state) e validada: carrega, pagina 1→2, busca fluida.
- Regra reutilizável: (1) toda listagem de coleção PocketBase deve nascer paginada (getList 20-50/página + busca/filtro server-side + contadores via 3 getList(1,1) agregados) — nunca getFullList em coleção que pode passar de ~500 registros; (2) após refatorar carregamento/renderização de uma tela, revalidar TODOS os estados no navegador (vazio, com dados, paginação, busca, modais) antes de pedir teste humano — o compilador não pega referência a variável removida dentro de JSX condicional.
- Quando aplicar: todas as listas do projeto (produtos, oportunidades, transbordo, timeline) desde a criação; qualquer refatoração de tela.
- Quando não aplicar: coleções garantidamente pequenas e fixas (ex.: 5 modelos de negócio).
- Confiança: alta — medido na base real (15.370 leads) e bug de tela reproduzido/corrigido/validado no navegador.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
