# Roteiro do teste do Bloco 2 da Fase 2 (owner — ~15 min)

**Versão**: 0.0.63 · **Data**: 13/11/2026 · **Preparado por**: Bart (insumos prontos na base)

## Preparado pelo Bart (não precisa criar nada)

- **"PROVA Teste Bloco 2 Baixa"** — oportunidade trilha baixa (Shockwatch), etapa 1 "Problema Emergente", **fechamento já salvo** (Venda em reais · BRL · AHM Brasil · 100.000). Serve para ganho/perda/reabertura.
- **"PROVA Teste Bloco 2 Alta"** — oportunidade trilha alta (PROVA Pipeline Alta), **já avançada até a etapa 6 "Implementação"**, fechamento salvo (Exportação EUA · USD · AHM Solution Americas · 80.000). Serve para o checklist com alerta.

## Parte 1 — Fechamento ganho/perda (F2-T06) — ~5 min

1. **Oportunidades** → abrir a oportunidade do lead **"PROVA Teste Bloco 2 Baixa"**.
2. Clicar **Registrar ganho** SEM motivo → **esperado**: bloqueio "Informe o motivo do ganho."
3. Informar o motivo (ex.: "Cliente fechou o piloto") e registrar → **esperado**: selo "Ganha" com data; o cartão "Próximo passo" muda para "Oportunidade ganha…".
4. Tentar mudar de etapa → **esperado**: bloqueio "Esta oportunidade está fechada. Reabra-a para mudar de etapa."
5. **Reabrir** sem justificativa → bloqueio; com justificativa (ex.: "Cliente pediu revisão do escopo") → reabre.
6. **Registrar perda** com motivo e concorrente → **esperado**: selo "Perdida".
7. Conferir o **Histórico de fechamentos** no modal: 3 registros (fechamento ganha, reabertura, fechamento perdida) — nada apagado.

## Parte 2 — Checklist com alerta (F2-T05) — ~5 min

1. Abrir a oportunidade do lead **"PROVA Teste Bloco 2 Alta"** (está na etapa 6 "Implementação").
2. Clicar **Concluir etapa** → abre o checklist da etapa: critérios de conclusão + sinais de insucesso.
3. Marcar 2 sinais de insucesso (ex.: "sem instruções de uso" e "sem alinhamento com as demais equipes").
4. Confirmar → **esperado**: alerta "Atenção: sinais de insucesso presentes" com botão **"Avançar mesmo assim para 'Realidade'"**.
5. Confirmar de novo → avança para "Realidade"; abrir o **Histórico** e conferir que a etapa 6 tem as marcações com data e responsável.
6. **Voltar etapa** para a 6 → tentar responder o checklist de novo → **esperado**: bloqueio "Esta etapa já foi encerrada: o checklist não pode mais ser alterado."

## Parte 3 — Painel de taxas (F2-T07) — ~3 min

1. **Métricas** → seção **"Pipeline — taxas de passagem"**: duas tabelas (AHM Cycle e Site Assessment) com Entraram, Avançaram, Real × Alvo; a última etapa mostra "—" (não tem taxa).
2. **Taxa de vitória** no topo: hoje "—" (sem fechamentos reais) com a meta 40% — os fechamentos da Parte 1 são em lead PROVA e saem junto na limpeza.
3. **Carregar demonstração** → as tabelas passam a incluir os dados DEMO; **Remover demonstração** → volta ao estado anterior (a remoção apaga só registros DEMO/PROVA).

## Parte 4 — Arquivamento (proteção) — ~2 min

1. **Leads** → abrir um lead de teste (ex.: "PROVA Teste Bloco 2 Baixa") em **Editar** → botão **Arquivar** com motivo → **esperado**: o lead sai da lista e dos contadores.
2. Alternar **Ver arquivados** → o lead aparece → **Restaurar** → volta para a lista.
3. Não existe mais botão de excluir lead real — a exclusão física só existe para registros PROVA/DEMO (controlada pelo servidor, com auditoria).

## Limpeza

- O Bart apaga só os registros **PROVA** criados no teste (2 leads + 2 oportunidades + marcações de checklist) e confirma a base real intacta.

## Resultado

- Tudo conforme → responder "bloco 2 ok" ao Bart.
- Qualquer desvio → descrever o passo e o comportamento observado.
