# Changelog

- 2026-11-12 · [Afonso] · Incidente: reimportação da base RD criou 7.548 leads duplicados (noite 12/11). A prévia sinalizou as duplicidades (comportamento correto do Bloco 4) mas a importação não bloqueia por design (RN-1.5: avisa e o usuário decide). Causa da lentidão sentida: importação sequencial (1 requisição por lead, ~31 ms/lead → ~4 min para 7,5 mil) + prévia de 15 mil contatos caindo no caminho da varredura (~2 s, aceitável). LIMPEZA concluída pelo Bart (script com rate-limit, ~40 min): 7.548 duplicados apagados com verificação por registro (só pendente sem produto); base final 15.370 leads, registros reais intactos (Eduardo Zuin Aprovada, Aroldo Rejeitada com 3 interações, 2 transbordos). Melhoria registrada: importação em lote otimizada (criação paralela no navegador, ~12× mais rápido — AP-2026-11-10-1626) para a próxima fase.

- 2026-11-12 · [Afonso] · Bloco 4 (pendências técnicas) IMPLEMENTADO pelo Claude Code (0.0.55-0.0.56), REVISADO pelo Bart: (1) DUPLICIDADE DA IMPORTAÇÃO CONTRA A BASE COMPLETA — o defeito (comparação só com a página visível de 20 leads) corrigido no service leadsService.chavesExistentesNaBase: até 300 contatos distintos → consultas em lote de 30 contatos por filtro OR (máx. 10 lotes em paralelo, paginação de 500 por lote); acima de 300 → varredura paginada com fields=contato,empresa, perPage=1000, skipTotal (~7-16 requisições para 15 mil); comparação final exata no cliente (contato+empresa, case-insensitive, trim); falha na consulta → prévia abre com aviso "Duplicidades não verificadas", nada gravado. (2) CAMPO TELEFONE — migration 0023_leads_telefone (text opcional, down remove), importação mapeia Telefone/Telefone principal/Telefone fixo/Fone/Phone/Phone number → telefone e Celular/Telefone celular/Mobile/WhatsApp → celular (telefone principal; na falta, o celular; os dois diferentes → "Celular: …" nas observações), campo Telefone no Novo/Editar Lead com placeholder "(11) 98765-4321" (limpar envia string vazia), telefone visível no modal da oportunidade com ícone Phone ("Nenhum contato registrado" só quando faltam e-mail+telefone+observações). Mudança fora do briefing justificada: placeholder do Contato no Novo Lead → "E-mail do contato" (evita competir com o campo novo). 30 testes automáticos (6 novos de importação; passaram fora do Skip). Provas do Bart (API + tela): telefone gravado e limpo via string vazia ✓; lead antigo da página 500 (LEANDRO MARTIR DE JESUS/Samarco) encontrado por lote (42 ms, 1 requisição) e por varredura (7 requisições, 963 ms) ✓; importação com Telefone/Celular: só Telefone → telefone gravado, Tel+Cel diferentes → telefone principal + "Celular: …" nas observações, só Celular → celular como telefone ✓; prévia de importação na TELA sinalizou 1 duplicada na base (lead antigo fora da página 1) com banner de aviso e nada gravado ✓; telefone do lead visível no modal da oportunidade (expand lead.telefone) ✓; linguagem sem códigos internos ✓. Base real intacta — registros de prova (6 leads + 1 oportunidade) apagados no fim.

- 2026-11-12 · [Afonso] · Blocos 2 e 3 CONCLUÍDOS: teste conjunto aprovado pelo owner ("Feito!" 12/11 ~19:05, roteiro 06_notas/roteiro-teste-blocos-2-3.md). F1-T19..T21 (SPEC 1.7 — timeline de interações, proposta, cancelamento auditável) e F1-T16..T18 (SPEC 1.6 — métricas, painel, demonstração) concluídas. Fase 1: 21/21 (100%) nas tasks de construção; pendências técnicas restantes entram no Bloco 4. Emenda 1.3a e conjunto de referência publicados (9e5683c/b3491d5) como PROPOSTA — tasks só após ratificação (owner + consultora).

- 2026-11-12 · [Afonso] · Bloco 3 + Bloco técnico (F1-T16+T17+T18) IMPLEMENTADO pelo Claude Code (0.0.52-0.0.54), REVISADO pelo Bart: migration 0022 (qualificacao_origem), tela /metricas (Qualificação automática com amostra mínima e aviso; Interações até a proposta; Capacidade por vendedor; filtros período/fonte; Copiar resumo), rota GET /backend/v1/metricas/capacidade (agregação no servidor, valida dias/fonte com 400), demonstração Carregar/Remover (10 leads DEMO, hooks geram 7 oportunidades + 1 transbordo, limpeza em cascata), 24 testes vitest, funções puras em metricas.ts, Oportunidades paralelizado. Provas: rota 401/400 ✓, 4×4 períodos×fontes ✓, K1 real 0% com aviso ✓, demo via botão 70%/4/10 ✓, limpeza completa ✓.

- 2026-11-12 · [Afonso] · Bloco 2 (F1-T19+T20+T21) IMPLEMENTADO pelo Claude Code (0.0.51), REVISADO pelo Bart: migration 0021 coleção interacoes, hook on_interacoes_validate.js (data não futura, tipo válido, proposta exige observação, 1 proposta por lead, cancelamento exige motivo), service interacoes.ts, TimelineInteracoes.tsx no modal Editar Lead. Provas API 10/10. Insumo do teste preparado no lead Aroldo (3 interações).

- 2026-11-12 · [Afonso] · Bloco 1 CONCLUÍDO (F1-T13+F1-T14+F1-T15 — SPEC 1.5 completa): teste aprovado pelo owner. Lição AP-2026-11-12-1037: teste humano precisa de insumo preparado na base.

- 2026-11-12 · [Afonso] · Bloco 1 (F1-T14+F1-T15) IMPLEMENTADO pelo Claude Code (0.0.48-0.0.49), REVISADO pelo Bart: migration 0020_oportunidades_decisao, hook on_oportunidade_decisao.js (rejeição sem motivo → 400; rollback exige justificativa; auditoria a cada decisão), tela com botões de decisão + "Decidido em ... por ...". 4 desvios justificados e aceitos. Correção extra: e.next() faltando em 2 hooks (0.0.48/0.0.49). Provas API 7/7. Correção do Bart (0.0.50): status em maiúscula.

- 2026-11-12 · [Afonso] · Varredura de qualidade + desempenho (0.0.43-0.0.45): bundle 2.848→1.374 KB (-52%, lazy loading por tela). Dashboard sem dados fictícios. Sem códigos internos, plurais corretos, sem fallbacks hardcoded. 10 coleções auth-only.

- 2026-11-12 · [Afonso] · Regra de linguagem pt-BR corporativo (0.0.41/0.0.42): textos de usuário sem códigos internos e com maiúscula correta — corrigido na origem e nos dados existentes. Regra em CLAUDE.md e memória do Bart.

- 2026-11-12 · [Afonso] · Task F1-T13 correção UX (0.0.39/0.0.40): contato do lead no modal + linguagem humanizada.

- 2026-11-11 · [Afonso] · Task F1-T13 IMPLEMENTADA (0.0.38): fila de oportunidades (SPEC 1.5). Migration 0019, hook RN-1.13, tela /oportunidades. Provas API 11/11.

- 2026-11-11 · [Afonso] · Task F1-T12 CONCLUÍDA: resolução/reabertura de transbordo (SPEC 1.4). 0.0.36 + 0.0.37. Provas API 11/11 + tela. Bloco SPEC 1.4 completo (F1-T10+T11+T12).

- 2026-11-11 · [Afonso] · Task F1-T12 correção (0.0.37): situação do lead visível no modal (feedback do teste humano).

- 2026-11-11 · [Afonso] · Task F1-T12 IMPLEMENTADA (0.0.36): resolução devolve lead a pendente com justificativa auditável; reabertura com motivo. Provas API 11/11 + tela.

- 2026-11-11 · [Afonso] · Task F1-T11 CONCLUÍDA: filtro por status na /transbordos (0.0.34-0.0.35). Provas API 10/10. Correção 0.0.35 (AP-2026-11-11-0545).

- 2026-11-11 · [Afonso] · Task F1-T11 IMPLEMENTADA: filtro por status na fila de transbordo. Provas API 10/10 + tela.

- 2026-11-10 · [Afonso] · Task F1-T10 CONCLUÍDA: fila de transbordo (SPEC 1.4). 0.0.29-0.0.30 + 0.0.33 (acentos — AP-2026-11-10-2149). Provas API 12/12 + teste aprovado.

- 2026-11-10 · [Afonso] · Task F1-T10 IMPLEMENTADA: migration 0018, hooks RN-1.11/RN-1.12, criação automática na qualificação, tela /transbordos. Provas API 12/12.

## 2026-09-17

- Pasta operacional inicial criada para a Fase 1.
