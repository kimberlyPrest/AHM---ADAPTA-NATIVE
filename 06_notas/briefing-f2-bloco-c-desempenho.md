# Briefing — Bloco C da Fase 2: Desempenho + Minha mesa + ajustes do pipeline (F2-T13)

**Data**: 13/11/2026 (noite) · **Autorização**: owner autorizou os 6 pontos · **Prioridade**: alta — fecha a Fase 2 e dá ao time a tela de trabalho diária.

**Contexto**: Blocos A (0.0.68) e B (0.0.69) aprovados pelo Bart (32/32 provas + tela). Owner respondeu os 6 itens da proposta do CC. Importação do histórico do Pipedrive vira **bloco próprio (D)**, logo depois — não entra aqui.

## Escopo

### 1. Tela "Desempenho" (réplica dos relatórios do Pipedrive)
Replicar os relatórios que o owner usa hoje no Pipedrive, alimentados pelas oportunidades do CRM:
- **Taxas de passagem** por etapa (já existe do Bloco 2 — integrar à tela)
- **Taxa de vitória** real × meta (40%)
- **Motivos de perda** — gráfico com a lista fixa abaixo (a mesma do Pipedrive, extraída dos relatórios que o owner mandou)
- **Negócios criados por mês** × meta
- **Totais por moeda** (R$ e US$ separados — nunca somar)
- **Por responsável** (produtividade por vendedor, quando houver mais de um dono)
- Estado vazio honesto: com poucas oportunidades reais (hoje: 2), os gráficos nascem vazios com mensagem clara ("sem dados suficientes no período") — **nunca inventar número ou frase** (regra do owner: dado inventado é defeito)

### 2. Lista fixa de motivos de perda (extraída dos relatórios do Pipedrive do owner — 31 motivos)
Substituir o campo livre `resultado_motivo` por select com estas opções (manter campo de observação livre ao lado):

**Alto volume**: Desistência · Sem retorno do cliente · Curiosidade · Duplicidade · Não consegui contato · Qualificação inadequada MKT
**Técnicas/comerciais**: Inviabilidade técnica · Preço · Optou por outra solução · Suspenso pelo cliente · Cliente do cliente · Empresa não está no momento · Projeto não aprovado · Características técnicas · Oferta de fornecimento · Prazo de entrega · Aprovação financeira · Barreira tecnológica
**Menores volume**: Decisão gerencial · Política de priorização · Solução do concorrente · Dados de contato incorretos · Percepção de valor agregado · Prazo de implantação · Relacionamento com fornecedor atual

- **Migração/backfill**: as oportunidades perdidas existentes (poucas) mantêm o motivo antigo no campo de observação; o select começa vazio nelas (não inventar classificação retroativa).
- Novos fechamentos exigem motivo da lista (o campo livre vira observação opcional).

### 3. Campo "Responsável" na oportunidade
- Campo novo `responsavel` (relation → users), **preenchido automaticamente a partir do lead** na criação da oportunidade (herda o responsável do lead).
- Editável pelo admin; vendedor vê (não edita o de outro).
- Alimenta os relatórios por responsável e a "Minha mesa".

### 4. Meta de negócios criados
- **Meta do time: 20 negócios criados por mês** (número do owner; ajustável depois em configuração simples do admin — não hardcoded).
- Gráfico "criados no mês × meta" na tela Desempenho.

### 5. Etapa de negociação (para os relatórios de funil)
- Trilha alta: **"Negociação (prova de conceito)"** (ordem 5)
- Trilha baixa: **"Proposta Customizada"** (ordem 3)
- Usar essas etapas como referência de "em negociação" nos relatórios.

### 6. Moedas separadas
- Todos os totais e gráficos de valor: **R$ e US$ em séries separadas**, nunca somados.
- Já existe moeda/entidade derivadas no fechamento (Bloco 1) — usar esse campo.

### 7. Tela inicial "Minha mesa" (no lugar do portal de tickets)
- Nova tela inicial do app: leads do vendedor (novos/pendentes), oportunidades dele (próximo passo em destaque), transbordos abertos, resumo do mês dele.
- Admin vê a mesma mesa com visão do time.
- **Menu**: Tickets e Atendimentos **saem do menu principal** (telas mantidas nas rotas para histórico/links antigos, sem destaque). Menu final: Minha mesa · Leads · Oportunidades · Transbordos · Métricas · Desempenho · Produtos · Equipe (só admin).

### 8. Correção pequena (achado dos logs)
- A tela da oportunidade só pede o histórico de fechamentos quando o usuário é admin (hoje o vendedor pede e leva 403 — requisição inútil + erro no log, visto em uso real às 20:53).

## Fora do escopo (Bloco D, próximo)
- Importação do histórico do Pipedrive (oportunidades fechadas, motivos, valores) — **com deduplicação contra a base** (lição do Bloco 5: varredura determinística + conferência de contagem antes de gravar) e mapeamento dos campos Pipedrive → CRM.

## Pré-mortem do Bart (3 jeitos de quebrar em produção)
1. **Lista fixa de motivos bloquear fechamento legítimo** (motivo real que não está na lista) → prova: fechar com cada motivo da lista funciona; opção "Outro" com observação obrigatória? — **decidir com o owner antes de implementar** (recomendo "Outro (especificar)" como 32ª opção com observação obrigatória).
2. **Responsável herdado do lead vazio** (lead sem responsável → oportunidade sem dono → relatórios com "sem responsável") → prova: oportunidade criada de lead sem responsável aparece no relatório como "sem responsável" (estado honesto, não inventado).
3. **Moedas somadas por descuido num gráfico** → prova: oportunidade em USD não entra no total em R$; conferir na tela com insumos nas duas moedas.

## Critérios de aceite (provas do Bart)
1. Desempenho: taxas de passagem + vitória + motivos de perda + criados×meta + moedas separadas + por responsável — cálculo replicado por API e conferido na tela.
2. Fechamento com motivo da lista (e "Outro", se aprovado) grava e aparece no gráfico; observação livre mantida.
3. Responsável herdado do lead; editável por admin; relatório por responsável correto.
4. Minha mesa como tela inicial; menu sem Tickets/Atendimentos; rotas antigas não quebram.
5. Vendedor não dispara requisição de histórico de fechamentos (logs limpos).
6. Regressão por papel em todas as telas (admin × vendedor) + varredura de dado inventado (nenhuma frase/número que não venha do banco).
7. Logs sem erro novo inesperado após a rodada de provas.
