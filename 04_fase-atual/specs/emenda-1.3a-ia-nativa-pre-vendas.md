# EMENDA 1.3a — AHM nativa de IA: pré-qualificação, recomendação técnica e proposta assistidas por IA

**Data:** 2026-11-12
**Origem do sinal:** Afonso Moreira (owner AHM Solution) — sessão com o Claude Code em 12/11/2026: objetivo declarado de tornar a AHM uma empresa **nativa de IA, começando por vendas**; leads chegam por site, landing page e WhatsApp; a IA qualifica pelas regras do owner e acelera pré-qualificação, recomendação técnica, proposta e previsibilidade de faturamento.
**Status:** PROPOSTA — decisões 1 a 4 da seção 9 já tomadas pelo owner; o restante aguarda ratificação (owner + consultora).
**SPECs afetadas:** SPEC 1.2 (entrada de leads), SPEC 1.3 (motor de qualificação — núcleo desta emenda), SPEC 1.4 (transbordo), SPEC 1.5 (oportunidades), SPEC 1.6 (KPIs), SPEC 1.7 (interações/proposta). Altera o sequenciamento da emenda 1.2a (módulos M4/M5).
**Relação com documentos anteriores:** não revoga nenhuma entrega. Revê o princípio da Fase 1 ("sem API") e antecipa a inteligência (M5) e o agente (M4-A2) para o núcleo do M1.

---

## 1. Diagnóstico que motiva a emenda

| Achado (verificado no código em 12/11) | Consequência para o objetivo |
|---|---|
| O motor de qualificação roda no **navegador** e só quando alguém clica em "Qualificar" (`src/lib/qualificacao.ts`); o servidor apenas confere o resultado. | Toda qualificação é intervenção humana — o K1 (≥ 70% sem intervenção) é inatingível pela arquitetura. Os 15 mil leads importados (13 mil rascunhos) ficam parados. |
| "Qualificado" = produto ativo + dor preenchida + urgência preenchida. Transbordo = 7 palavras-chave fixas no código. | O motor não lê o conteúdo da dor nem usa o conhecimento do catálogo. Regra nova exige task de desenvolvimento. |
| A IA nativa do Skip (`$ai.chat`, agentes hospedados com base de conhecimento) está disponível e **não é usada** em nenhuma tela. | O roadmap da emenda 1.2a coloca IA no M4/M5 (Fase 3/4) — a ordem é inversa ao objetivo. |
| As 11 coleções de negócio têm a mesma regra de acesso: qualquer usuário autenticado lê, altera e apaga qualquer registro. | Inviável antes de abrir canais externos e equipe com dados reais de clientes. |
| O QA do Skip roda sem suíte de testes (`test: ran false`). | As provas são manuais a cada versão — origem das "voltas". |

## 2. Decisão de produto

**A IA faz o trabalho; o humano supervisiona as exceções.** O agente de pré-qualificação é o produto central do M1; o CRM é a memória e o painel de controle dele.

**Divisão de responsabilidades (inegociável):**

1. **IA entende e extrai** — lê a mensagem/formulário em linguagem natural, preenche campos estruturados e formula as próximas perguntas.
2. **Regras decidem** — o status (qualificado / pendente / fora do recorte / transbordo) é decidido por regras **declarativas, editáveis pelo owner**, e explicado.
3. **Humano aprova as exceções** — transbordo, oportunidade e proposta passam por aprovação humana (mantém D-05 e D-06 do escopo definitivo).

**Salvaguardas obrigatórias do agente:**

- Nunca inventa preço, prazo ou especificação técnica. Preço vem do ERP/tabela; especificação vem da base de conhecimento.
- Toda recomendação técnica **cita a fonte** (ficha, FAQ, família de diagnóstico). Sem fonte na base → transbordo para especialista.
- Nunca contata o cliente sem aprovação humana (D-06).
- Toda saída da IA é gravada com a entrada, a versão das regras e a fonte — auditável como o resto do sistema.

## 3. Princípio da Fase 1 — revisão

**Antes:** "nenhuma task depende de API, credencial, webhook, conector ou integração externa".

**Depois:** "nenhuma task depende de **credencial, sistema ou decisão de terceiros**". Ficam permitidos:

- a IA nativa do Skip (`$ai.chat`, `$ai.embed`, agentes hospedados) — **decisão do owner em 12/11**;
- um ponto de entrada próprio (endpoint do próprio sistema) para receber leads.

Continuam fora da Fase 1 tudo que exige credencial de terceiros: Envya, RD Station, Pipedrive, OMIE, QuickBooks, provedor de WhatsApp.

## 4. Arquitetura alvo

| Componente | O que é | Observação |
|---|---|---|
| **Entrada única** | Endpoint próprio que recebe lead de qualquer canal (formulário do site, landing page, Envya, CSV) e cria lead + conversa. | Envya **mantido** como bot de atendimento (decisão do owner); na Fase 2 ele entrega a conversa a este endpoint. Proteção anti-spam e limite de requisições obrigatórios. |
| **Conversa** | Cada lead nasce como conversa: mensagens recebidas, perguntas feitas, respostas extraídas. | Substitui ou reaproveita as coleções `tickets`/`atendimentos` herdadas do template (decidir na Fatia 3). |
| **Base de conhecimento** | Catálogo, ICP, FAQ, perguntas, 8 famílias de diagnóstico, Site Assessment, fichas técnicas, casos anteriores. | Memória do agente (RAG). PDFs entram pela interface do Skip; o restante por migration ou sincronização com a coleção `produtos`. |
| **Regras declarativas** | Perguntas obrigatórias por produto/família, critérios (BANT), pesos, gatilhos de transbordo, recorte piloto. | Cadastro editável pelo owner em tela. Substitui as regras escritas dentro de `qualificacao.ts`. Versionado (cada qualificação guarda a versão usada). |
| **Motor no servidor** | Executa as regras automaticamente a cada lead criado, importado ou atualizado. | O botão "Qualificar" vira "Requalificar". Uma única implementação, testada. |
| **Agente de pré-qualificação** | Extração estruturada (`$ai.chat` com chamada de ferramenta) + recomendação técnica com citação (agente hospedado com RAG). | Modo sugestão primeiro: grava sugestões; a regra decide; o humano vê a divergência. |
| **Painel do vendedor** | Fila única priorizada (score × urgência × valor estimado) com o "por quê" e a próxima ação sugerida. | Evolução da fila de oportunidades. |
| **Rascunho de proposta** | Modelo por produto × modelo de negócio (Cíclico, Assinatura, Locação, Pontual, Consultoria), preenchido pela qualificação. | Vendedor revisa e aprova. Preço do ERP (contrato a definir no M4). |
| **Eventos de integração** | Cada mudança de estado relevante gera um evento registrado (lead criado, qualificado, oportunidade ganha…). | RD, Envya, OMIE e o ERP consomem eventos — integração nova não mexe no núcleo. |
| **Dados para previsão** | Etapa, probabilidade, valor, ambiente de fechamento, ciclo por modelo de negócio, timing de recompra. | Modelo de dados desenhado desde já para o forecast do ERP (emenda 1.2a §2). |

## 5. Conjunto de referência (como medimos "assertivo")

O owner monta **30 a 50 casos reais** do histórico com a resposta certa: status esperado, produto, família, perguntas que faltariam e motivo de transbordo. Modelo do arquivo: `04_fase-atual/conjunto-referencia/`.

- Toda mudança em regras ou no agente roda contra o conjunto e informa a assertividade ("acertou 46 de 50") — vira teste automático do QA.
- O teste humano do owner passa a ser **revisar apenas as divergências**, não clicar em telas.
- Metas iniciais propostas (owner ratifica): status ≥ 85%; produto ≥ 80%; **zero** caso de transbordo esperado classificado como qualificado (erro grave).
- Casos com dados pessoais reais: anonimizar nome e contato antes de entrar no repositório.

## 6. Base técnica obrigatória antes de canais externos

1. **Permissões por papel** (vendedor, especialista, admin) — regras de acesso por coleção revisadas; apagar registro restrito ao admin.
2. **LGPD** — base legal e consentimento na entrada, retenção definida para conversas, dados pessoais fora do conjunto de referência.
3. **Entrada protegida** — anti-spam, limite de requisições, validação de origem.
4. **Testes automáticos** — motor de regras, hooks críticos e conjunto de referência rodando no QA do Skip a cada versão.
5. **Ambientes** — preview para desenvolvimento e prova; produção publicada só após o portão do owner.

## 7. Novo sequenciamento (fatias verticais)

Cada fatia entrega um fluxo ponta a ponta, com dado de demonstração e roteiro de teste do owner já incluídos.

| Fatia | Entrega | Depende de |
|---|---|---|
| **0 — Prova técnica da IA** (curta) | Testar `$ai.chat`/agente com 10 casos do conjunto de referência: qualidade da extração, latência, custo por lead. Resultado decide `fast` × `reasoning`. | 10 casos do owner |
| **1 — Motor no servidor + regras editáveis** | Qualificação automática a cada lead; cadastro de perguntas/critérios/gatilhos por produto; testes automáticos; importação CSV já sai qualificada. | Fatia 0 (opcional) |
| **2 — Pré-qualificação com IA (modo sugestão)** | Agente lê texto livre (dor, observações, CSV) e sugere produto, dor normalizada, urgência e perguntas faltantes; aplicado aos 13 mil rascunhos em lote controlado. | Fatias 0 e 1 |
| **3 — Entrada única + conversa** | Endpoint próprio para site/landing; lead nasce como conversa e é pré-qualificado na chegada. Permissões por papel e LGPD entram aqui. | Fatia 2 |
| **4 — Recomendação técnica com citação + rascunho de proposta** | Recomendação ancorada na base de conhecimento; proposta gerada por modelo; aprovação humana. | Fatia 3; modelos de proposta do owner |
| **5 — Envya → entrada única** (Fase 2) | Conversas do Envya chegam ao sistema e seguem o mesmo fluxo. | API/credencial do Envya |

**Painel K1/K2/K3 (F1-T16 a T18):** mantido e antecipado — mede o "antes" e prova o ganho de cada fatia.

## 8. Impacto nas tasks existentes

- **Nada é descartado.** Catálogo, leads, motor auditável, transbordo, oportunidades e interações viram a estrutura que o agente usa.
- **F1-T01 a F1-T15:** concluídas/em teste — permanecem.
- **F1-T19 a F1-T21 (Bloco 2):** seguem para revisão do Bart; a timeline de interações alimenta o K2.
- **F1-T16 a F1-T18:** mantidas, prioridade alta.
- **Pausados:** novos refinamentos de telas manuais que o agente vai assumir, até a Fatia 2.
- **`src/lib/qualificacao.ts`:** substituído pelo motor no servidor na Fatia 1 (as regras atuais migram para o cadastro declarativo como versão inicial).

## 9. Decisões

| # | Decisão | Status |
|---|---|---|
| 1 | Permitir a IA nativa do Skip no projeto (revisão do princípio da Fase 1, §3). | **Decidido pelo owner — 12/11/2026** |
| 2 | Manter o Envya como bot de atendimento por enquanto; ele alimentará a entrada única na Fase 2. | **Decidido pelo owner — 12/11/2026** |
| 3 | Pipedrive na transição (pendência 4 da emenda 1.2a). | Owner de acordo em tratar na transição; data de saída e eventual operação em paralelo a definir. |
| 4 | Owner monta o conjunto de referência (30 a 50 casos). | **Aceito pelo owner — 12/11/2026** |
| 5 | Ratificação desta emenda e do novo sequenciamento (§7). | Pendente — owner + consultora |
| 6 | Metas de assertividade (§5). | Pendente — owner |
| 7 | Destino das coleções `tickets`/`atendimentos` do template. | Pendente — decidir na Fatia 3 |

## 10. Riscos e incertezas

- **Custo e cotas da IA do Skip** não estão documentados nos guias da plataforma — medir na Fatia 0 antes de processar os 13 mil rascunhos.
- **Qualidade da extração em pt-BR técnico** — medida pelo conjunto de referência; sem o conjunto, não há como afirmar assertividade.
- **Base de conhecimento incompleta** gera mais transbordo, não resposta errada (salvaguarda §2) — completar fichas por família reduz o transbordo.
- **Envya**: disponibilidade e formato da API ainda não verificados.
