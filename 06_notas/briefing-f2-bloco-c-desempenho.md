# Briefing — Bloco C: Desempenho + Minha mesa (F2-T13/T14)

> **CORREÇÃO (13/11 noite, Bart)**: o título original deste item dizia "31 motivos" por erro de contagem meu — a lista sempre teve **25**. Os 25 corretos entraram na migration 0033 + "Outro (especificar)" inativo. Revisão do Bloco C em 06_notas/revisao-bloco-c-provas-bart.md (APROVADO).

## 1. Tela inicial "Minha mesa" (no lugar do portal de tickets)
- Nova rota `/` com: saudação, botão "Novo lead", resumo do mês (criados c/ meta, em negociação, ganhos, abertos — valores R$ e US$ separados), oportunidades abertas c/ próximo passo, leads para trabalhar, transbordos abertos
- Vendedor vê o que é dele; admin vê o time
- **Menu final**: Minha mesa · Leads · Oportunidades · Transbordos · Métricas · Desempenho · Produtos · Equipe (só admin)
- Tickets e Atendimentos saem do menu; rotas continuam funcionando (histórico/links antigos)

## 2. Lista fixa de motivos de perda (extraída dos relatórios do Pipedrive do owner — 25 motivos; título anterior dizia 31 por erro de contagem do Bart, corrigido 13/11 noite)
Substituir o campo livre `resultado_motivo` por select com estas opções (manter campo de observação livre ao lado):

**Alto volume**: Desistência · Sem retorno do cliente · Curiosidade · Duplicidade · Não consegui contato · Qualificação inadequada MKT
**Técnicas/comerciais**: Inviabilidade técnica · Preço · Optou por outra solução · Suspenso pelo cliente · Cliente do cliente · Empresa não está no momento · Projeto não aprovado · Características técnicas · Oferta de fornecimento · Prazo de entrega · Aprovação financeira · Barreira tecnológica
**Menores volume**: Decisão gerencial · Política de priorização · Solução do concorrente · Dados de contato incorretos · Percepção de valor agregado · Prazo de implantação · Relacionamento com fornecedor atual

- **Migração/backfill**: as oportunidades perdidas existentes (poucas) mantêm o motivo antigo no campo de observação; o select começa vazio nelas (não inventar classificação retroativa).
- Novos fechamentos exigem motivo da lista (o campo livre vira observação opcional).

## 3. Campo "Responsável" na oportunidade
- Campo novo `responsavel` (relation → users), **preenchido automaticamente a partir do lead** na criação da oportunidade (herda o responsável do lead).
- Editável pelo admin; vendedor vê (não edita o de outro).
- Alimenta os relatórios por responsável e a "Minha mesa".

## 4. Meta de negócios criados
- **Meta do time: 20 negócios criados por mês** (número do owner; ajustável depois em configuração simples do admin — não hardcoded).
- Gráfico "criados no mês × meta" na tela Desempenho.

## 5. Etapa de negociação (para os relatórios de funil)
- Trilha alta: **"Negociação (prova de conceito)"** (ordem 5)
- Trilha baixa: **"Proposta Customizada"** (ordem 3)
- Usar essas etapas como referência de "em negociação" nos relatórios.

## 6. Moedas separadas
- Todos os totais e gráficos de valor: **R$ e US$ em séries separadas**, nunca somados.
- Já existe moeda/entidade derivadas no fechamento (Bloco 1) — usar esse campo.

## 7. Tela "Desempenho" (réplica dos relatórios do Pipedrive)
- Filtros: período, vendedor, trilha. Vendedor vê os próprios números; admin vê o time todo (e pode filtrar por vendedor ou "sem responsável").
- Destaques: em negociação, abertas, ganhas, taxa de vitória × meta 40%, ticket médio, tempo até ganhar.
- Gráficos: criados por mês × meta, ganhos por mês, abertas por etapa (negociação em destaque), funil pré-venda → comercial, rosca ganhas × perdidas, perdas por motivo, perdas por etapa, tempo médio por etapa.
- Tabela por responsável (só admin).
- Sem dados → "Sem dados no período" (nunca número inventado). Dados de demonstração excluídos.
- **Configurações** (botão só admin dentro de Desempenho): editar meta mensal, ativar/desativar motivos de perda.

## 8. Importação do histórico do Pipedrive (bloco próprio, separado)
- Importação do histórico do Pipedrive (oportunidades fechadas, motivos, valores) — **com deduplicação contra a base** (lição do Bloco 5: varredura determinística + conferência de contagem antes de gravar) e mapeamento dos campos Pipedrive → CRM.

## Correções de erros de logs incluídas
- Vendedor não pede mais o histórico de fechamentos (restrito a admin) ao abrir oportunidade.
