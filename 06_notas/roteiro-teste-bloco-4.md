# Roteiro de teste do owner — Bloco 4 (0.0.57)

**Tempo estimado:** 10 minutos. **Onde:** preview do sistema, logado com o seu usuário.
**O que está sendo testado:** duplicidade da importação contra a base inteira + campo telefone no lead.

---

## Parte 1 — Duplicidade contra a base inteira (arquivo: `insumo-teste-bloco-4-duplicado.csv`)

1. Em **Leads**, clique em **Importar CSV** e escolha o arquivo `insumo-teste-bloco-4-duplicado.csv` (uma linha só: o lead real Aroldo Teles da Silva, que está na base mas **fora da página 1**).
2. **Esperado na prévia:** o aviso amarelo "Possíveis duplicidades: **1 já existe na base** (mesmo contato+empresa)" e o contador **1 Duplicadas**.
3. **Clique em Cancelar** — nada deve ser gravado (o Aroldo já existe; importar criaria um duplicado).

## Parte 2 — Telefone na importação (arquivo: `insumo-teste-bloco-4-telefone.csv`)

4. Clique em **Importar CSV** e escolha `insumo-teste-bloco-4-telefone.csv` (linha nova, com Telefone e Celular diferentes).
5. **Esperado na prévia:** **0 Duplicadas** (contato novo) e **1 Válida**. Confirme a importação.
6. Abra o lead **Prova Bloco Quatro** (botão **Editar**): o campo **Telefone** deve mostrar `(11) 5555-0000` e as observações devem trazer `Celular: (11) 94444-5555`.
7. Ainda no Editar, **apague o telefone** e salve: reabrindo, o campo deve estar vazio (a limpeza grava string vazia).

## Parte 3 — Telefone no modal da oportunidade

8. Em **Oportunidades**, abra a oportunidade **Sugerida** do lead **PROVA Bloco4 Tel** (se ainda existir) ou de qualquer lead qualificado com telefone: o telefone deve aparecer na seção **Contato**, ao lado do e-mail.

## Encerramento

9. Me avise no chat do Bart ("bloco 4 ok" ou o que falhou) — eu apago o lead de prova **Prova Bloco Quatro** e concluo a task. Com a sua aprovação, a **Fase 1 fecha 21/21**.
