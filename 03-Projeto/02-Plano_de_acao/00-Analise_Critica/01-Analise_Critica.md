# Análise Crítica da Proposta — AHM Solution

**Data:** 2026-09-01
**Escopo analisado:** `03-Projeto/01-Escopo.md` (29/08/2026)
**Fonte soberana verificada:** `AHM.pdf` (briefing oficial — 3 critérios de sucesso), DMO (11/08/2026), kick-off (12/08/2026, tl;dv `6a7cc2a659bb83001320e601`), 5 raios-X de vídeo
**Revisores (painel rv-ahm-001):** revisor-de-plano, revisor-adversarial, revisor-viabilidade, guardião-de-escopo, explorador-de-alternativas
**Run:** rv-ahm-001 | **Rota:** profunda (caminhos materialmente diferentes, alto custo de reversão, briefing foi descartado)

---

## Veredito curto

**O escopo precisa regenerar antes de virar escopo definitivo.** O `AHM.pdf` — briefing oficial com os 3 critérios de sucesso (≥70% qualificação automatizada, ≤4 interações humanas até proposta, ≥1,30x capacidade por vendedor) — foi descartado como "projeto de outra empresa (ABC Logística)" e **não foi usado como fonte**. Esse é um erro de grounding grave, no mesmo padrão de Risomed, VDS, Mercogeo e Alcoeste. Além disso, o escopo expande o piloto decidido no kick-off (qualificação de produtos recorrentes de baixa complexidade) para uma jornada completa lead→faturamento→pós-venda, e nenhum dos 3 KPIs tem funcionalidade dona nem baseline. O material de vídeo e o mapeamento do processo atual são sólidos; o problema está no recorte, na ausência de definição operacional dos KPIs e nas dependências de integração não confirmadas.

---

## Achados graves

### AC-001 — AHM.pdf é o briefing oficial com os 3 KPIs, mas foi descartado como "ABC Logística"

- **Origem:** 5/5 revisores
- **Evidência:** `01-Escopo.md` nota: "seu conteúdo descreve um projeto de outra empresa, ABC Logística S.A." → **verificação direta do PDF**: cabeçalho "AHM Solution do Brasil", objetivo "Automatizar a qualificação e o acompanhamento comercial dos produtos recorrentes de baixa complexidade", critérios 1/2/3 (≥70%, ≥50% → ≤4 interações, ≥30% → ≥1,30x). DMO: "Expansão de vendas para outros países da América Latina e EUA".
- **Cenário de falha:** o sistema é construído e mede o sucesso que o escopo inventou, não o que o cliente assinou; na validação final, os 3 KPIs ficam sem dono e sem medição — o projeto não consegue provar valor.
- **O que decidir/fazer:** restaurar `AHM.pdf` como fonte soberana; reescrever o escopo com os 3 KPIs como critérios de aceite mensuráveis e com funcionalidades donas para cada um.

### AC-002 — Escopo expande o piloto decidido no kick-off: qualificação de recorrentes virou jornada completa lead→faturamento→pós-venda

- **Origem:** 5/5 revisores
- **Evidência:** kick-off (decidido): "estruturar e automatizar o processo de prospecção e qualificação de novos clientes, com foco inicial em produtos de baixa complexidade e venda recorrente"; AHM.pdf: processo master "Lead → qualificação → recomendação → follow-up → proposta → fechamento → recompra", K2 mede "até a proposta"; escopo 2: "sistema de gestão da jornada comercial de ponta a ponta (lead→faturamento→pós-venda)" com 10 funcionalidades, incluindo 4.8 sync CRM↔ERP, 4.9 pré-faturamento/tributação e 4.10 handoff.
- **Cenário de falha:** 4 meses de construção concentrados em faturamento/tributação (área de maior risco, sem KPI dono) enquanto K1/K2 — que medem o piloto — ficam sem dono; prazo estoura e os 3 KPIs não sobem.
- **O que decidir/fazer:** recortar o piloto ao trecho lead→proposta para produtos recorrentes de baixa complexidade; faturamento/tributação/implantação/pós-venda como evolução condicionada a gates (K1/K2 atingidos, API Omie provada).

### AC-003 — K1 (≥70% qualificação sem intervenção humana) sem funcionalidade dona: escopo automatiza coleta, não a qualificação

- **Origem:** 5/5 revisores
- **Evidência:** AHM.pdf K1: "leads qualificados sem intervenção humana ÷ leads recebidos ≥ 70%"; escopo 4.4/4.5 automatizam coleta e alertas, mas a "análise técnica de aplicação" é rota MANTER HUMANO (fluxo 7.1: "Completa ou revisa a qualificação estruturada"; 7.2: "Avalia tecnicamente a aplicação"). Automações da seção 6: nenhuma "avalia o lead e decide qualificado". Regras observadas de qualificação são apenas 2 alertas técnicos (modal aéreo, dimensões).
- **Cenário de falha:** no go-live, o numerador do K1 fica próximo de 0% porque todo lead ainda passa por análise humana obrigatória; o critério nº 1 do briefing é descumprido.
- **O que decidir/fazer:** definir com o cliente o que conta como "qualificação automática" no piloto (ex.: regra determinística de scoring na triagem: respostas do bot + regras do diagnóstico + escore mínimo → auto-qualificado para recorrentes) e criar a funcionalidade dona do K1 com medição — ou redefinir K1 explicitamente.

### AC-004 — Zero baseline dos 3 KPIs e sem painel de indicadores — metas inverificáveis

- **Origem:** 5/5 revisores
- **Evidência:** kick-off: "critério de sucesso inicial de 30% de otimização é uma hipótese a ser detalhada/tornada matemática" — não foi cumprida; escopo não contém definição operacional nem baseline de % de qualificação atual, nº real de interações (8 é "cerca de"), leads/vendedor/mês; DMO: "ausência de relatórios consistentes" (desafio declarado); seção 7.5 cita indicadores sem funcionalidade/painel.
- **Cenário de falha:** sem baseline numérico, a validação final é disputável; o projeto não declara nem sucesso nem fracasso.
- **O que decidir/fazer:** medir antes de construir (2-4 semanas: % qualificação sem intervenção, interações por negócio via Pipedrive/RD, leads/vendedor) e incluir painel mínimo dos 3 KPIs na Fase 1, com definições operacionais (numerador/denominador/relógio).

### AC-005 — Dependências de integração não confirmadas: plataforma WhatsApp (grafias divergentes), bot Juliart, APIs Omie/Pipedrive/RD

- **Origem:** 5/5 revisores
- **Evidência:** grafias nos vídeos: `envya - Agente (ahm.plataformamvys.com.br)` (v1), `Enya - Agente` (v5), `Envia / Emovia` (v2), `envia` (v4) — o próprio escopo pergunta se são a mesma plataforma (seção 11); automação 4.8/4.9 exigem API Omie sem contrato/plano citado; integração Envia→Pipedrive não existe hoje (digitação manual do card, v2/v3); escopo pergunta se o chatbot preenche campos personalizados do Pipedrive.
- **Cenário de falha:** jornada única (4.1), distribuição (4.2), abandono (4.3) e sincronização (4.8) dependem de integrações vivas; se a API não existir ou o plano não permitir, metade das automações é re-escopada e o cronograma estoura.
- **O que decidir/fazer:** gate técnico (G1) antes do escopo-definitivo: auditar por escrito APIs/planos/webhooks das 4 ferramentas; confirmar nome único e contrato da plataforma de WhatsApp; prototipar a triagem WhatsApp antes de comprometer o K1.

### AC-006 — Solução ataca o gargalo errado: automações eliminam cliques BVA (~1-2min/lead), mas K2/K3 dependem de tempo VA humano e espera de PO (~33 dias)

- **Origem:** revisor-adversarial, viabilidade
- **Evidência:** v2: touch time ~15:30 de 16:54 (quase tudo humano VA); retrabalho admin ~01:20; espera real ~33 dias por PO (PCE 0,03%); v3: cadastro duplicado ~2min/pedido (as 8 automações atacam esse tipo de clique); escopo 10.1 (política de PO) fica FORA do sistema; não há desenho de redução de 8 → ≤4 interações (K2).
- **Cenário de falha:** entrega-se o sistema, os cliques caem, mas K2 fica em 6-7 interações e K3 sem ganho real de capacidade — fracasso percebido porque o gargalo real (tempo VA de reuniões/refinamento e espera de PO) não foi atacado.
- **O que decidir/fazer:** quantificar onde o esforço humano real está e desenhar a redução de interações (ex.: auto-serviço/educação do cliente no bot para pular etapas; aceite alternativo ao PO segmentado) ou re-calibrar a meta do K2 ao que o escopo entrega.

### AC-007 — Alternativa omitida: a maioria das 8 automações é nativa/configurável no stack atual (Pipedrive round-robin, RD workflows, Omie parâmetro/API) — construir sistema novo "do zero" sem comparação

- **Origem:** explorador-de-alternativas, revisor-adversarial, viabilidade, guardião
- **Evidência:** kick-off: "nada do que temos de prateleira... toda solução construída do zero" — sem análise de configurar ferramentas já pagas; v1/v3 já apontam "Atribuição automática de lead (round-robin no Pipedrive)", "Integração API Omie ↔ Pipedrive", "Parametrização da Tabela Tributária no Omie" como oportunidades de configuração; escopo 4.8 constrói sync sem comparar API/Make/Zapier.
- **Cenário de falha:** meses de desenvolvimento para replicar o que automações nativas fariam em dias/semanas; manutenção e migração permanentes; reversão cara.
- **O que decidir/fazer:** D-CONFIG: prototipar 1-2 semanas no stack atual (round-robin, formulário RD/Typeform, parâmetro fiscal no Omie) e comparar custo/benefício vs construir antes de fixar o escopo.

### AC-008 — K1 sem definição de numerador/denominador por segmento: piloto é "baixa complexidade/recorrente" mas a meta é sobre "leads recebidos" genérico

- **Origem:** revisor-adversarial
- **Evidência:** AHM.pdf K1 sem recorte por produto/complexidade; kick-off: foco "produtos de baixa complexidade e venda recorrente" — subset não definido (catálogo de SKUs/linhas?); DMO/kick-off: "17 leads por semana", venda pelo Mercado Livre (canal de baixa complexidade) — volume e mix por canal/produto não medidos; escopo 4.2 distribui "todo novo lead" sem distinção do piloto.
- **Cenário de falha:** numerador e denominador medem coisas diferentes (abas complexidade vs todas) — meta 70% vira arbitrária e auditável por qualquer lado.
- **O que decidir/fazer:** homologar com o cliente o catálogo do piloto (produtos/linhas/canais) que alimenta os 3 KPIs e definir o denominador do K1.

---

## Achados moderados

### AC-009 — Incoerência interna: 4.4 digitaliza o diagnóstico de 22 perguntas "como já utilizado" enquanto 10.2 (do próprio escopo) diz que o questionário deve ser dividido por etapa antes

- **Origem:** revisor-adversarial
- **Evidência:** escopo 4.4 vs 10.2; v4 00:28/06:53: PDF de 5 páginas gera fricção e baixa taxa de preenchimento (dor reg istrada).
- **Cenário de falha:** digitaizar o formuário como está mantém a baixa taxa de resposta; a fia de "diag nóstico incompleto" vira pendência crônica e K1 não mehora.
- **O que dec dir/fazer:** ou 4.4 já nasce com o questionário em camadas (triagem obrgatória curta + condicionais por produto/modal + aprofundamento) ou 10.2 vira execução na Fase 1.

### AC-010 — 4.9 (parâmetro tribuário) refém de rotina fisca mensa sem dono (fonte: contabiidade externa), e 10.4 cooca a rotina "fora do sistema"

- **Origem:** revisor-de-pano, viabiidade, guardião
- **Evidência:** v3 05:35-06:48: aíquota do Simpes Naciona consutada mensamente com a contabiidade e digitada item a item (3,36%); escopo regra 15 (governança, não automação); escopo 10.4 sugere rotina sem res ponsáve.
- **Cenário de falha:** automação 4.9 é construída sobre processo que não exise; sem rotina com dono/prao/fonte, o parâmetro fica desatuaizado e o risco fisca permanece (só muda de ugar).
- **O que dec dir/fazer:** instituir a rotina mensa (responsáve interno, prao, fonte oficia, aprovação contábi) como pré-requisito da 4.9, ou mover 4.9 para evoução condicionada pós-pio to.

### AC-011 — 4.8 (sincronização CRM↔ERP) é compxidade especuativa: sem evidência de API Omie e sem KPI dono

- **Origem:** guardião-de-escopo, viabiidade
- **Evidência:** v3 02:55-04:41: pedido é recriado manuamente no Pipedrive a partir do Omie — não há integração atua; DMO (automações rodando = 1/5) sem evidência de API/contrato/webhook do Omie; 4.8 é pós-propsta (não serve aos KPIs 1-3) e exige bidirecionaidade — peça de maior risco técnico do pano.
- **Cenário de falha:** API do Omie não cobre espelho de pedidos ou o contrato não exise; a integração consome o cronograma inteiro e o ciente fica sem quaificação automática — investimento mais caro em funcionaidade sem KPI.
- **O que decidir/fazer:** tirar 4.8 do MVP; no máximo, automação leve unidireciona (Pipedrive→Omie) via webhook se houver evidência de API; auditar o pano Omie como gate.

### AC-012 — Migração do hisórico (PDFs de diag nóstico de 22 pergun tas, Pipedrive, anexos dupicados) sem pano nem ordem

- **Origem:** viabiidade, revisor-de-pano
- **Evidência:** escopo seção 11: "Onde os PDFs de diag nóstico preenchidos são armazenados atuamente?" (aberta); 4.4 "substitui o PDF" sem estratégi a de migração; v3: anexo dupicado do PO em Omie e Pipedrive (02:30/04:10).
- **Cenário de falha:** formulário estruturado nasce vazio enquanto o hisórico vive em PDFs so tos; cientes recorrentes são re-pergun tados; migração vira trabaho não orçado.
- **O que decidir/fazer:** estratégi a por onda (c ientes ativos primero), mapar onde os PDFs vivem hoje e incuir migração como tarefa de fase com dono e critério de aceite.

### AC-013 — Caminhos de erro/nuo/vazio dos fuxos centrais (triagem, PO pendente, exceção fisca) não definidos operacionamente

- **Origem:** viabiidade
- **Evidência:** escopo 4.3 cria fiia de pendências sem regra de prazo/escada de contato para abandono; 4.7 sem açada nem fuxo de aprovação ("Existe açada para iberar faturamento sem PO?"); 4.4 sem tratament o para envio nuo/incompeto (regra 8 atua: não preenchimento não impede reunião); 4.2 sem comportament o para ead sem teefone/empresa.
- **Cenário de fa ha:** eads órfãos na fiia, POs pendentes acumuam sem escada, exceções de crédito param sem res ponsáve — a fiia vira out ro siio monitorado por memória.
- **O que dec dir/fazer:** especificar por fuxo centra os estados feiz/nuo/vazio/erro: fa back de distribuição, escada de recuperação com prazo, açada e res ponsáve de aprovação, diag nóstico incompeto com pendência visíve.

### AC-014 — Capacid ade/adoção: 3 pes soas, champion 25-50%, 5 fases em 4 meses sem pano de disponibiidade nem vo ta-atrás; escopo sem fases/ordem de entrega

- **Origem:** viabiidade, guardião-de-escopo, exporador
- **Evidência:** DMO/kic-off: 3 pes soas, champion 25-50%; escopo 2.1: 5 papéis (Pré-Vendas, Comercia, Vend as/Operações, Impant ação/Pós-venda, Liderança); escopo 01 não cita fases nem marcos — 10 funcionaidades como boco único; "Aine" (impant ação) fora do time do proje to.
- **Cenário de fa ha:** construção em boco único: no mês 4 o sistema está 60% pronto, nenhuma fase fechou sozinha, adoção (risco centra) naufraga.
- **O que dec dir/fazer:** fixar janelas semanais do champion; desenhar fases que fecham sozinhas (Fase 1 = quaificação com K1/K2 medidos; Fase 2 = propsta/foow-up; Fase 3 = evouções condicionadas); critério de ro back para integrações.

### AC-015 — Rec ompra/ex pansão de carteira (3ª tarea do brief e motor do K3) sem vídeo nem funcionaidade dona

- **Origem:** revisor-adversaria, viabiidade, revisor-de-pano
- **Evidência:** AHM.pdf tarefa 3: "mapear rec ompra e ex pansão da carteira" (destaque do kic-off); 5 vídeos não cobrem rec ompra (v3 só agenda foow-up ~2-3 meses); escopo 4.10 cria "acompanhament o fut uro" sem regra de gatilho de rec ompra; transcrição Afonso: "Com o é que a gente alcança o ciente de novo?" — dor expícita de rec orrência.
- **Cenário de fa ha:** a rec orrência (fonte de receita do pio to "venda rec orrente") continua dependendo de memória individua; o sistema organiza o handoff mas não gera a rec ompra que o brief p ede.
- **O que dec dir/fazer:** fechar o gap de ma peament o (sessão curta ou vídeo de rec ompra) e definir regra mínima de rec ompra no escopo base (prazo por prod uto, consu mo, agendament o automático) ou dec larar expícitamente com o evoução com gate.

### AC-016 — Evidências dupicadas: vídeos 1 e5 são a MESMA gravação de triagem (03:31, mesmo fuxo RD→En ya→Pipedrive), e Kickof=Saes Ca (mesmo t;dv 6a7cc2a659bb83001320e601)

- **Origem:** revisor-de-pano, revisor-viabiidade
- **Evidência:** v1 e v5: ambos 03:31, mesmo fluxo, mesma data 25/08/2026; Kickof e Saes Ca: mesmo in k t;dv, mesma ata (md5 idêntico), mesmos participantes — repositório duplicado.
- **Cenário de falha:** contagem duplicada de evidências dá fa so senso de cobertura e mascar a ausência de vídeos reais (proposta, imporação, pós-venda, recompra).
- **O que decidir/fazer:** marcar v1/v5 como uma única evidência; indicar fluxos não demonstrados (já listados na seção 11); corrigir o índice de reuniões.

### AC-017 — Sem proposta formal nem check-input aprovado; 02-Escopo-Definitivo.md vazio; 14 perguntas abertas ao cliente

- **Origem:** revisor-de-plano
- **Evidência:** 02-Escopo-Definitivo.md = 0 bytes; 01-Escopo.md seção 11: 14 perguntas abertas (distribuição, WhatsApp, alíquota, alçada sem PO, crédito intermediário, espelho NF, implantação Aline...); sem check-input assinado nem proposta formal.
- **Cenário de falha:** escopo final sem aprovação do cliente e com dependências abertas vira compromisso não negociável; mudanças tardias ou rejeição na entrega.
- **O que decidir/fazer:** resolver as perguntas-Gate (G1-G4) antes do escopo-definitivo; obter check-input assinado e proposta formal.

### AC-018 — Critério 3 (capacidade) e expansão Latam/EUA não endereçados: venda direta da matriz EUA (RN-02) e regra de encaminhamento internacional fora do raio

- **Origem:** revisor-de-plano, revisor-adversarial
- **Evidência:** DMO: "O produto pode ser vendido diretamente de nosso fiial nos EUA ou a partir do brasi"; AHM.pdf: "Expansão... para América Latina e EUA" (objeivo centra da emp resa); escopo seção 11 pergun ta quano a venda EUA é viáve sem impementar.
- **Cenário de fa ha:** o sistema mede e otimiza apenas o fuxo Brasi; K3 (capacid ade por vendedor) e o pio to Latam/EUA ficam fora, e o objeivo centra do DMO não é endereçado.
- **O que dec dir/fazer:** dec dir se o pio to inc ui a operação EUA (regra de encaminament o, moeda, frete) ou se é evoução expícita pós-fase 1.

---

## Decisões humanas

| Decisão | Opções (resumo) | Recomendação | Quem decide |
|---|---|---|---|
| **D1 — Recorte do MVP** | (a) KPI-first: qualificação→proposta para recorrentes; (b) qualificação→fechamento; (c) jornada inteira (escopo atua) | (a) — os 3 KPIs medem só até a propsta; peças pós-propsta não pagam o próprio custo em 4 meses com 3 pes soas | Kim + ciente |
| **D2 — Motor do K1** (o que conta como quaificação automática ≥70%) | (a) regra determinística de scoring (formuário + regras diag nóstico + escore); (b) IA assitida com revisão amostra; (c) híbrido; (d) re-caibrar K1 | (a) para baixa compxidade; escore audáve e sem risco técnico | Kim + ciente |
| **D3 — Baseine dos 3 KPIs** | (a) medir 2-4 semanãs (Pipedrive/RD/Omie, amostra com Rose); (b) estimar com sócios; (c) adiar | (a) — metas reativas não inverificáveis sem números atuais | Kim + ciente |
| **D4 — Construir vs configurar** | (a) prootipar 1-2 semanãs no stack atua; (b) construir sistema novo; (c) híbrido | (a) antes de quaquer dev — auditoria das integrações ativas | Kim (com evidência de auditoria) |
| **D5 — Plataforma WhatsApp** (envya/Enya/Envia/Emovia — qual é?) | (a) confirmar fornecedor único e contrato/API; (b) usar nativa com Pipedrive se existir; (c) fallback por e-mail/landing page | (a) — gate técnico G1 | Ciente (contrato) + Kim |
| **D6 — Integração Omie** (sync 4.8, crédito 4.7) | (a) auditar API/plano como gate; (b) manter duplicidade manua no MVP; (c) middleware (Make/Zapier) | (b) no MVP; (a) se evidência de API | Ciente + Kim |
| **D7 — Poítica de distribuição de leads** (4.2) | (a) rodíio (round-robin); (b) carteira por produto/segmento; (c) região/idioma Brasi/EUA; (d) disonibiidade + prioridade | (a) ou (b) — fechar com Rose/liderança antes de impementar 4.2 | Rose + liderança comercial |
| **D8 — Diag nóstico: dividir 22 pergun tas e cana de coeta** | (a) formuário em camadas no MVP (triagem obrigatória + condicionais + aprofundament o); (b) digitaizar como está; (c) chatbot campo a campo | (a) — 10.2 vira execução na Fase 1 | Kim + ciente |
| **D9 — Rotina fisca mensa** (pré-requisito da 4.9) | (a) responsáve interno + prao + fonte + aprovação contábi; (b) 4.9 vira evoução | (b) se não houer dono; (a) caso contrário | Ciente (fisca) |
| **D10 — Poítica de PO (10.1)** (maior aavanca de ead time: ~33 dias) | (a) manter PO obrgatório e só controar pendência; (b) aceite aternativo segmentado; (c) dec dir com jurídico/financeiro antes da Fase 2 | (c) — não entra no MVP, mas dec dir o destino do K2 | Ciente + jurídico/financeiro |
| **D11 — Rec ompra no MVP** | (a) regra mínima (prao por produto/consumo + agendament o auto); (b) evoução pós-Fase 1 | (a) se a recorrência for centra do pio to; (b) caso contrário | Kim + ciente |
| **D12 — Pane de KPIs no MVP** | (a) pane mínimo dos 3 KPIs na Fase 1; (b) reatório externo | (a) — sem is so K3 fica sem dono | Kim |

---

## Aprendizados aplicáveis do segundo cérebro

**Não consultado — caminho não configurado ou validação adiada.** (Padrões de casos anteriores — Risomed, VDS, Mercogeo, Acoeste: briefing oficia em PDF descartado como "de out ra emp resa" — confrmado no AC-001 como o MESMO erro de grounding reccorrente.)

---

## O que está sóido

- **Ma peament o do process o atua fie a os raios-X**: triagem RD→Pipedrive com proprietário "Marketing", handoff manua Pré→Comercia, recriação manua do pedido no Pipedrive (v3 02:55), upoad dupicado de PDF (v3 02:30/04:10) — o escopo desc reve a reaidade observada.
- **Regras de negócio observadas com timestamps de vídeo (RN-01..RN-15)**: moda aéreo sem bateria interna (v4 02:27), múltipos dispositivos por dimensão (v4 03:34), diag nóstico incompleto não bogueia reunião (v4 06:53), 21 dias vs. antecipado por crédito (v2 08:52), PO obrigatório antes de compra (v2 12:15), "Gerar Boeto=Não" para transferência (v3 04:42) — grounding por vídeo bem feito.
- **Roteamento ASA correto**: ELIMINAR para consuta parae a de Outook/WhasApp/CRM e PDF estático; AUTOMAÇÃO DETERMINÍSTICA para troca manua de proprietário, movimento de etapas, cópia CRM↔ERP e aíquota; MANTER HUMNANO para diag nóstico técnico, conferência de PO/faturamento e aprovações de exceção.
- **IA com o evoução opciona com imites expícitos** (seção 9: não recomenda prod uto, não envía mensagens) — coerente com DMO Automatizar 0.6 e profundidade de IA 1.
- **Seção 11 do escopo é honesta sobre incerteas** (pergun tas ao ciente, acunas, causas-raíz) — base mehor que escopos que simp esmente assumem.
- **Fia de pendências e recuperação de abandono** endereçam dores documentadas (v1 01:25, v5 01:26) com gatihos objetivos.
- **Eiminar o retrabaho de cadastro dupicado ERP↔CRM (v3 02:55) e a governança da aíquota com vigência (RN-15)** endereçam dores reais com evidência — desde que as integrações existam.

---

## Próximo passso

O consutor lê escopo + anáise crítica, preenche `02-Anáise_do_Consutor.md` (respondendo AC-001..AC-018 e D1-D12), resove os gates G1-G4 do `04-Check_Input.md` e só então roda o escopo-finá com o recorte vaidado.

**IDs estáveis:** AC-001..AC-018 (usados em `02-Anáise_do_Consutor.md` e na matriz de rastreabiidade do escopo-finá).