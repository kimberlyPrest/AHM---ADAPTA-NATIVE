# Briefing — Fase 2, Bloco 2 (proteção contra exclusão + F2-T05 + F2-T06 + F2-T07)

**Data:** 2026-11-13 · **Autor:** Bart (revisão/provas) · **Para:** Claude Code (implementação)
**Autorização:** o owner autoriza a implementação DEPOIS de aprovar o teste do Bloco 1 (roteiro 06_notas/roteiro-teste-f2-bloco1.md). Aguardar o "bloco 1 ok".

## Item 1 — Proteção contra exclusão na base real (pedido do owner, 13/11 ~05:53)

**Contexto:** o incidente das 00:15 UTC de 13/11 (DELETE no lead real Aroldo por script de limpeza com filtro malformado) mostrou que um lead sem vínculos — como a maioria dos 15.370 — teria sido apagado. O owner quer proteção NO SERVIDOR, não dependendo de disciplina de script.

**O CC propõe a melhor opção entre as duas (ou a combinação delas) e justifica no relatório de implementação:**

- **Opção A — restrição de papel:** apagar leads, oportunidades, transbordos e interações fica restrito a administrador (API rules por coleção; usuários comuns recebem 403).
- **Opção B — arquivamento:** a exclusão comum é substituída por arquivamento — o registro sai das listas (filtro de status), mas continua na base; exclusão física só para administrador.

**Requisitos obrigatórios (valem para as duas opções):**

1. **Limpeza de prova controlada pelo servidor:** o servidor só aceita apagar registros marcados como prova — prefixo **PROVA** ou **DEMO** no nome/título. Qualquer exclusão de registro NÃO marcado como prova, vinda de script ou de usuário comum, é recusada (mensagem corporativa pt-BR).
2. **A demonstração do painel de Métricas continua funcionando:** carregar e remover a demonstração (leads DEMO + oportunidades + transbordo + interações DEMO) segue operante — a remoção da demo apaga registros DEMO (autorizados pelo marcador).
3. **A limpeza de provas do Bart continua funcionando:** os registros PROVA criados nos testes continuam apagáveis (marcador PROVA), mantendo o fluxo de revisão atual.
4. **Registros reais ficam intocáveis por script:** Eduardo Zuin, Aroldo, transbordos reais, oportunidades reais — nenhum script os apaga; se um dia precisarem ser removidos, é ação manual do administrador na tela, com auditoria.
5. **Hook de requisição** (não model hook) para ter e.auth e o papel do usuário; mensagem de recusa clara ("Este registro não pode ser excluído por não ser um registro de prova." / "A exclusão deste registro é restrita ao administrador.").
6. **Auditoria:** toda exclusão autorizada (PROVA/DEMO ou admin) registra quem apagou, o quê e quando (padrão etapa_por/interações).

**Provas do Bart:** script comum tenta apagar lead real → recusado; tenta apagar lead PROVA → aceito; usuário comum tenta apagar → 403/recusa; demo carregar/remover segue funcionando ponta a ponta; base real intacta.

## Item 2 — F2-T05: Checklist por etapa com alerta de insucessos (emenda 1.2b RN-2.4)

- Fonte canônica: emenda 1.1a §4 + consolidação do Manual de Vendas 05 (aprovada pelo owner em 09/11 22:05).
- **Checklist de critérios de conclusão por etapa** exibido na troca de etapa (modal da oportunidade, dentro do passo a passo): as perguntas de reflexão viram campos do registro da etapa; os fatores de sucesso viram critérios de conclusão.
- **Lista de insucessos vira alerta automático:** ao marcar a etapa como concluída, se algum sinal de insucesso estiver presente, a tela mostra o alerta antes de confirmar (ex.: etapa 6 "sem instruções de uso / não sabe a meta / sem alinhamento com demais equipes / não gera entregável").
- Cadastro das etapas já existe (pipeline_etapas, 13 registros) — o checklist pode ser coleção nova `etapa_checklists` (etapa, ordem, texto, tipo: criterio|insucesso|pergunta) com seed das 13 etapas, ou campo JSON na pipeline_etapas — o CC escolhe e justifica.
- Marcações ficam no registro do histórico (oportunidade_etapas): checklist respondido por quem, quando — auditável.
- Linguagem: sem códigos internos nas telas.

**Provas do Bart:** checklist visível na troca de etapa nas duas trilhas; alerta de insucesso dispara; marcações auditáveis; 13 etapas com seed correto.

## Item 3 — F2-T06: Proposta e fechamento ganho/perda com motivo auditável (SPEC 1.8)

- Fechamento da oportunidade: **ganha** ou **perde**, com **motivo obrigatório** auditável (quem, quando, motivo — sem apagar).
- **Aqui entra a validação pendente do Bloco 1:** ganho/perda EXIGE ambiente de fechamento + valor estimado > 0 (mesma validação da troca de etapa).
- Campos sugeridos: resultado (ganho/perda), motivo (text), concorrente (text opcional), data do fechamento. Ganho alimenta win rate (F2-T07) e, no futuro, a entrada no ERP (emenda 1.2a §2).
- Cancelamento/reabertura do fechamento: auditável, sem apagar o registro anterior (padrão das interações).
- Linguagem: pt-BR corporativo.

**Provas do Bart:** ganho sem motivo → 400; perda sem motivo → 400; ganho sem fechamento/valor → bloqueio; auditoria completa; reabertura preserva histórico.

## Item 4 — F2-T07: Painel de taxas de passagem real × alvo (emenda 1.2b RN-2.3)

- Painel compara, por etapa das DUAS trilhas: **taxa de passagem real** (quantos % das oportunidades que entraram na etapa avançaram) × **taxa-alvo** da emenda 1.1a (alta: 60/70/80/90/95/100/100/100; baixa: 60/70/90/100/100).
- Estado sem dados: mensagem clara (padrão do painel de Métricas), sem erro.
- Win rate aparece quando houver fechamento (Item 3) — alvo 40% (Bíblia de Vendas; a validar na SPEC 1.6 emenda).
- Filtros por período, coerentes com o painel atual; fonte dos dados = oportunidade_etapas (histórico com datas).

**Provas do Bart:** painel com dados de prova (oportunidades PROVA avançadas) mostra real × alvo; sem dados → estado vazio claro; soma/contagens fecham com o histórico.

## Regras transversais (valem para tudo)

- Linguagem pt-BR corporativo em toda string de UI (sem códigos internos RN-x.x/F2-Txx).
- Migrações 4 dígitos sequenciais (0027+); hooks de REQUISIÇÃO quando precisar de auth/papel.
- Base real intacta: 15.379 leads (15.370 reais + 9 do teste do owner), Eduardo Zuin qualificado, Aroldo pendente c/ 3 interações, 2 transbordos, 2 oportunidades reais.
- Testes automáticos na pasta de testes, rodados à parte (QA do Skip não executa) — relatar o total.
- **NÃO iniciar antes do "bloco 1 ok" do owner.**
