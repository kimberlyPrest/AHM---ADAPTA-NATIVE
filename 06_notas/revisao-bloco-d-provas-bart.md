# Revisão do Bloco D (0.0.73) — provas do Bart · 14/11/2026

**Protocolo aplicado**: pré-mortem com 3 riscos provados · regressão por papel · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

## Resultado: APROVADO — provas (a)-(f) todas verdes na 1ª rodada; importação real liberada para o owner após a conferência da amostra na prévia

## Contexto

Importação do histórico do Pipedrive com as 7 decisões do owner (análise em `06_notas/analise-bloco-d-pipedrive.md`): 10.906 negócios → 3.541 oportunidades + 4.753 leads + 2.612 fora (Conversa Abandonada/Testes). Arquitetura: mapeamento no navegador (`src/lib/pipedrive.ts`, com testes), rota `POST /backend/v1/pipedrive/importar` (só admin; `simular:true` não grava; gravação em lotes de 500; dedup por `pipedrive_id` + empresa+pessoa normalizados).

## Provas do pedido (com planilha PROVA de 6 itens, colunas da exportação do Pipedrive)

| Prova | Resultado |
|---|---|
| (a) Prévia → importar → repetir | ✅ **Prévia** (`simular:true`): 6 recebidos → 4 ops novas + 4 leads novos + 1 ligada a lead existente + 1 lead já na base (duplicado do lote), 0 falhas, **0 registros gravados** (conferido na base). **Importar**: mesmas contagens, 0 falhas. **Repetir**: 5 `jaImportados`, **0 novos** — dedup por `pipedrive_id` funciona; reimportação segura |
| (b) Vendedor chama a rota → 403 | ✅ "A importação do histórico do Pipedrive é restrita ao administrador." |
| (c) USD entra no total; EUR fora | ✅ Op ganha USD 50.000 → apareceu em `ganhas.USD` do Desempenho (qtdUSD 1). Op aberta EUR 7.000 → `valor_estimado 0`, `moeda ''`, **valor_original 7000 + moeda_original EUR guardados** — não entrou em nenhum total |
| (d) Op importada aberta sem ambiente: mudar etapa → bloqueio | ✅ 400 "Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento." (1ª tentativa devolveu 400 de trilha errada — erro do MEU teste, passei etapa da trilha baixa; re-testado com etapa da própria trilha alta) |
| (e) Criadas por mês usam a data do Pipedrive | ✅ Ops/leads com criado_em 2025-03, 2024-11, 2023-06 e 2024-02 → `porMes` mostra 1 criada em CADA um desses meses (não em 2026-11) |
| (f) Limpeza dos registros PROVA | ✅ 4 ops + 2 leads PROVA apagados (204); base com 0 registros `pipedrive_id~'PROVA-D'` |

## Comportamento verificado além do pedido

- **Responsáveis**: "Afonso Moreira" → admin · "Rose" → Rose (na9v1yo7bre4dhi) · "Julio" → Julio (voervp32o7igae0) · **"Vendedor Fantasma" → null (sem responsável)** — correspondência por nome funcionando, sem inventar vínculo
- **Lead novo importado**: status `pendente`, `incompleto: true`, origem "Importação Pipedrive", `pipedrive_etapa` = "[PD] Pré-vendas > Qualificação > Aberto", observação pedindo para completar o contato — nada inventado
- **Vínculo a lead existente**: op PROVA-D-104 ligada ao lead PROVA criado antes (chave empresa+pessoa normalizada); amostra da prévia mostra o par "Pipedrive → lead da base" para conferência do owner
- **Perda importada**: motivo da lista ("Preço") gravado como `motivo_perda`; lead pré-venda "Perdido" → arquivado com motivo
- **Duplicado no mesmo lote**: 2º lead com a mesma chave → `leadsJaNaBase`, não criou 2º registro

## Pré-mortem — 3 riscos

1. **Importação duplica ao repetir** → PROVADO: 2ª rodada = 5 jaImportados, 0 novos. ✓
2. **Moeda estrangeira vira valor no sistema** → PROVADO: EUR ficou só em valor_original/moeda_original; totais inalterados. ✓
3. **Dado inventado no lead importado** → PROVADO: rascunho/pendente sem contato, etapa PD registrada, observação explícita. ✓

## Riscos que encontrei além do pedido

1. **(baixa) Amostra de ligações limitada a 25** — a prévia mostra só as 25 primeiras ligações a leads existentes; com 3.541 ops, o owner confere uma amostra, não a lista toda. É o desenho combinado (amostragem), mas vale dizer em voz alta: a conferência dele é por amostra.
2. **(baixa) `usuarioDe` com nome ambíguo → sem responsável** — dois usuários com o mesmo primeiro nome e sobrenome não idêntico podem não casar (regra conservadora: só casa com candidato único). Comportamento correto (prefere não ligar a ligar errado), mas se o owner estranhar negócios "sem responsável" de vendedores cadastrados, é aqui.
3. **(nota) Falhas silenciosas por item** — item com erro entra em `falhas` (até 200) e a importação segue; a tela deve exibir as falhas com destaque (conferir na prévia real).
4. **(nota) `etapa_desde` de negócios antigos** = última alteração de etapa do Pipedrive (pode ser recente) — não afeta gráficos de tempo por etapa (que não ganham histórico de qualquer forma).

## Segurança

- Rota só admin (403 para vendedor provado; `$apis.requireAuth()` no middleware) ✓
- Planilha real fica em `05_dados/` (fora do GitHub, .gitignore criado pelo CC) — **nada da pasta foi ao repositório** ✓
- Nenhum dado real de cliente trafegou nas minhas provas (planilha PROVA sintética) ✓

## Logs

- Nenhum aviso de hook novo durante as provas (último warning e.next() segue 12/11 12:53) ✓

## Conclusão

**Bloco D APROVADO** (0.0.73): prévia sem gravação, dedup idempotente, moedas estrangeiras fora dos totais, bloqueio de etapa sem ambiente, datas originais nos gráficos, responsáveis mapeados com fallback conservador. **A importação real (10.906 negócios) pode ser executada pelo owner** — recomendo: rodar a prévia com a planilha real, conferir a amostra de ligações e as falhas, e só então confirmar. Desfazer exige procedimento especial (proteção do Bloco 2), então a conferência da prévia é o ponto sem volta.
