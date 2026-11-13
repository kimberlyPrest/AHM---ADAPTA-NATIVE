# Relatório — Revisão do Bloco 5 (provas do Bart)

**Data:** 13/11/2026 · **Versão provada:** 0.0.58 · **Revisor:** Bart · **Para:** Claude Code

## Veredito

Bloco 5 APROVADO com 2 defeitos a corrigir — um deles bloqueante para o objetivo "zero duplicados".

## Provas que passaram

1. **Critério 1 (reimportação da base RD completa, 15.372 linhas, TELA):** prévia correta "15.370 duplicadas / 2 novos" (os 2 eram leads de teste apagados entre tentativas); importação gravou só os 2; ZERO duplicados criados dos 15.370 existentes.
2. **Critério 2 (mesclado):** CSV de 5 linhas → prévia "3 novos / 2 duplicadas"; gravados só os 3 (1ª ocorrência da repetida interna); duplicada da base ignorada.
3. **Critério 4:** plural "1 já existe na base" / "15.370 já existem na base" ✓; relatório com milhar ✓; aviso "A importação gravará somente os leads novos. Os duplicados serão ignorados." ✓; botão "Importar N leads novos" ✓.
4. **Critério 5 (regressão):** telefone principal gravado, celular diferente → "Celular: …" nas observações, urgência Alta, cargo → observações ✓.
5. **Bloqueio por falha na verificação:** código conferido (banner vermelho + botão desabilitado); não reproduzido em tela (não há forma simples de forçar a falha).
6. **Velocidade:** 1.000 leads em ~24 s (10 paralelas, 0 falhas) → 7.500 ≈ 3 min no melhor caso. Meta < 1 min para 7.500 não estável — a latência por criação no servidor domina (AP-2026-11-12-2135).

## Defeitos encontrados

### D1 — Linha sem contato com nome+empresa de lead existente COM contato passa como nova (BLOQUEANTE)

- **Prova:** CSV com linha "Aroldo Teles da Silva;Stellantis (Mopar);" (sem contato) → classificada NOVA e gravada como rascunho, mesmo existindo 2 registros de Aroldo com contato na base.
- **Causa:** `chaveDuplicidade` gera `nome:aroldo teles da silva|stellantis (mopar)` para a linha, mas os registros da base (com contato) geram chave `contato|empresa` — nunca batem.
- **Correção sugerida (escolher uma):** (a) a varredura registra `nome:|empresa` como chave secundária de TODO registro da base (custo ~2× memória, simples); (b) linha sem contato tratada como inválida com motivo claro ("preencha o e-mail para verificar duplicidade"). Recomendo (a) + aviso na prévia.

### D2 — Varredura instável sob carga (causa-raiz não determinada; exigência de robustez)

- **Prova:** 1ª tentativa da reimportação completa → verificação retornou VAZIA (prévia: "15.370 NOVOS") e a importação criou ~882 duplicados antes de ser abortada e limpa. 2ª tentativa idêntica: 16 páginas, todas com status 200, classificação correta. Réplica manual via fetch (mesmos parâmetros): 15.373 itens, matching correto.
- **Suspeita:** paginação com `skipTotal` sob carga — página com 429/5xx (ou truncada) encerra o loop como se fosse a última (`if (r.items.length < 1000) break`), e a verificação "passa" com cobertura parcial.
- **Correção exigida:** varredura determinística — (1) retry com espera por página em 429/5xx; (2) conferência de cobertura (totalItems uma vez, ou exigir última página completa); (3) cobertura incompleta → tratar como falha de verificação (bloquear importação — mecanismo já existe). O mesmo padrão de loop existe no caminho de lotes (`if (r.items.length < 500) break`).

## Estado da base

15.370 leads; registros reais intactos (Eduardo Zuin Aprovada, Aroldo Rejeitada com 3 interações, 2 transbordos, 2 oportunidades). 1.005 registros de prova apagados.

## Prioridade

Corrigir D1 e D2 numa única versão 0.0.59. O Bart re-prova: critério 1 (reimportação completa), linha sem contato (D1) e simulação de falha de página (D2) antes do teste do owner.
