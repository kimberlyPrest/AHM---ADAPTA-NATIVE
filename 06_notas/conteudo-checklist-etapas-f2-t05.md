# Conteúdo do checklist por etapa (F2-T05) — fonte: Manual de Vendas 05 + Playbook Baixa Complexidade

**Data:** 2026-11-13 · **Preparado por:** Bart · **Para:** Claude Code (seed em migration 0030)
**Fontes:** Manual de Vendas 05 (mar/2020, `uploads/a946cdb5-Site_Assessment.pdf` — PDF escaneado, sem camada de texto; conteúdo conforme consolidação APROVADA pelo owner em 09/11/2026 22:05) e Playbook de Vendas Baixa Complexidade (abr/2026, `uploads/aaf2457e` — texto extraído).
**Formato:** cada item vira um registro em `etapa_checklist_itens` (etapa, ordem, tipo, texto). Tipos: `criterio` (fator de sucesso/critério de conclusão), `insucesso` (sinal que gera alerta), `pergunta` (pergunta de reflexão).
**Atenção:** os 4 insucessos da etapa 6 (alta) JÁ ESTÃO na base (migration 0028) — não duplicar. Responsável sugerido e duração das 8 etapas altas também já estão (0028).

## TRILHA ALTA — Site Assessment (8 etapas)

### Etapa 1 — Cenário Atual (Pré-venda)
- criterio: Indicação positiva do Cliente Ideal confirmada
- criterio: Contatos confirmados e registrados no CRM
- pergunta: A prospecção foi direta ou apoiada em relatórios de inteligência do negócio?

### Etapa 2 — Diagnóstico (Pré-venda)
- criterio: Cargo, área e papel do contato identificados
- criterio: Problema claro identificado
- criterio: Visita técnica agendada
- criterio: Tudo registrado no CRM
- pergunta: Qual é a dor do cliente? Onde dói?
- pergunta: O cliente consegue visualizar a solução ideal?
- pergunta: Existe algum impedimento técnico?

### Etapa 3 — Refinamento (visita técnica) (Pré-venda)
- criterio: Informações técnicas, comerciais e prazos confirmadas
- criterio: Objetivo e KPIs definidos
- criterio: Stakeholders, processos e concorrentes conhecidos
- criterio: Business Case construído (ROI e Break-Even)
- pergunta: O cliente tem experiências anteriores semelhantes?
- pergunta: Quanto estão dispostos a investir? Há valores para "testar a solução"?
- pergunta: Qual o impacto em dinheiro (economia)? Os parâmetros técnicos e prazos estão confirmados sem subjetividade?

### Etapa 4 — Apresentação de Projeto (Venda)
- criterio: Cliente e stakeholders convencidos
- criterio: Objeções combatidas
- criterio: Cliente projetando resultados
- criterio: Apresentação contém justificativas do projeto, o que o cliente quer, objetivo + KPI e prazo de implementação
- criterio: Ferramentas do decisor preparadas (4 Blocker, ROI & Breakeven, tempo de instalação, atuais clientes, comparativo técnico-comercial)
- pergunta: A equipe impactada está envolvida? O cliente está comprometido com a proposta?
- pergunta: O cliente tem opções para declinar? O que o tem motivado?
- pergunta: Como faço diferente dos concorrentes? Quem são os stakeholders?
- pergunta: Qual o benefício para o cliente? Qual o benefício legado único da ferramenta? Qual o produto e como funciona?

### Etapa 5 — Negociação (prova de conceito) (Venda)
- criterio: PoC validada
- criterio: Próximos passos confirmados
- criterio: Plano de ação claro e prazo de entrega acordados
- criterio: Padrão de linguagem sem suposições, julgamentos ou generalizações; Rapport estabelecido
- criterio: Confirmação técnica combinada (formulário e relatórios); definido quem faz instalação e manutenções
- pergunta: Metodologia aplicada (MAPAN/ZOPA, sinais de compra, gatilhos mentais, 4 estágios: Abertura, Investigação, Demonstração de capacidade, Obtenção de compromisso)?

### Etapa 6 — Implementação (Venda)
- criterio: Medidas adotadas e solução instalada
- criterio: Confirmação técnica registrada (formulário e relatórios)
- insucesso: Cliente sem instruções de uso da solução *(já cadastrado)*
- insucesso: Cliente não sabe qual é a meta da implementação *(já cadastrado)*
- insucesso: Sem alinhamento com as demais equipes do cliente *(já cadastrado)*
- insucesso: A implementação não gera um entregável *(já cadastrado)*

### Etapa 7 — Realidade (Pós-venda)
- criterio: Solução funcionando com indicadores mensuráveis
- criterio: Case coletado
- criterio: Referência comercial solicitada
- pergunta: O que amplia ou limita o êxito? Há oportunidades de cross-selling?

### Etapa 8 — Adoção Corporativa (Pós-venda)
- criterio: Cliente interessado em ampliar
- criterio: Storytelling formatado (WhitePaper/Infográfico)
- pergunta: O que funcionou, o que não funcionou, o que mudou?
- pergunta: O que o cliente sentiu de mais forte? O que mudou no relacionamento?

## TRILHA BAIXA — AHM Cycle (5 fases) — fonte: Playbook Baixa Complexidade (abr/2026)

### Fase 1 — Problema Emergente (Pré-venda)
- criterio: Cenário coletado (volume de envios, frequência de avarias, tipo de embalagem)
- criterio: Necessidade clara identificada e registrada no CRM
- pergunta: Qual o impacto anual das avarias, incluindo custos ocultos (seguros, retrabalho, reputação)?
- pergunta: Há pontos cegos ou quase-acidentes na operação?

### Fase 2 — Diagnóstico Técnico Compartilhado (Pré-venda)
- criterio: Necessidade validada com o cliente
- criterio: Solução apresentada de forma tangível (demonstração física ou imagens/QR)
- pergunta: Quais os desafios ou lacunas da solução atual (dados em tempo real, precisão)?
- pergunta: Há histórico de impactos (G-forces) ou perdas por temperatura/umidade?

### Fase 3 — Proposta Customizada (Venda)
- criterio: ROI simples calculado e apresentado ("Com X avarias, o produto economiza R$Y por mês")
- criterio: Piloto ou compra inicial de baixo risco proposto
- criterio: Todas as interações e o status registrados no CRM
- pergunta: O valor do teste está definido? O retorno paga o investimento em menos de um mês?

### Fase 4 — Implementação Piloto (Venda)
- criterio: Entrega realizada em até 7 dias
- criterio: Produto instalado e em uso correto confirmado
- criterio: Meta do piloto definida e conhecida pelo cliente
- insucesso: Cliente sem instruções de uso do produto
- insucesso: Piloto sem meta definida
- insucesso: Sem acompanhamento combinado com o cliente

### Fase 5 — Acompanhamento e Expansão (Pós-venda)
- criterio: Follow-up pós-venda realizado (satisfação e uso correto)
- criterio: Oportunidade de recompra/expansão identificada (PUSH 45–120 dias)
- pergunta: Os resultados estão conforme o esperado? Há necessidade de ampliar (mais unidades ou novas áreas)?

## Instruções para a migration (0030)

1. Seed dos itens acima em `etapa_checklist_itens`, vinculando cada item à etapa correspondente de `pipeline_etapas` (trilha + ordem), com `ordem` sequencial por etapa e o `tipo` indicado.
2. NÃO duplicar os 4 insucessos da etapa 6 alta (já existem da 0028) — apenas completar os critérios/perguntas que faltam nela.
3. Itens marcados "(já cadastrado)" servem só de conferência.
4. Down: apagar os itens criados (por texto exato ou por lista de ids capturados no up).
5. Depois do seed, conferir contagem por etapa via API e reportar.
