# Briefing — Bloco 4: pendências técnicas (última task da Fase 1)

**Quem executa:** Claude Code (sessão AHM, via MCP Skip) — escreve e roda o QA do Skip.
**Quem revisa:** Bart (Maestro) — provas via API, validação de tela, docs e GitHub. NÃO edite fase.md/STATUS/changelog.
**Coordenação:** um escritor por vez. Ao terminar, o webhook avisa o Bart automaticamente.

## Contexto (o que JÁ existe — não recriar)

- Coleção `leads`: nome, empresa, contato (e-mail), produto_interesse, dor, urgencia, origem, status, qualificacao_resultado/pendencias/historico, qualificado_em/por, qualificacao_origem (0022), observacoes (texto livre — hoje é onde telefone às vezes vai).
- Importação CSV (`src/lib/csvImport.ts` + tela Leads): prévia com válidas/inválidas/duplicadas no arquivo/duplicadas na base. **DEFEITO CONHECIDO:** a comparação "duplicadas na base" usa só os leads da página visível (`leads.map(...)` no componente — 20 registros), não a base completa de 15 mil.
- Modal Editar Lead: campos empresa/contato/dor/urgencia/produto/observacoes + TimelineInteracoes.
- Modal da oportunidade: seção Contato mostra `lead.contato` (e-mail) e `lead.observacoes`.
- Versão corrente: 0.0.54. Base real: NÃO apagar Eduardo Zuin/Arkema, Aroldo Teles da Silva (Stellantis), os 4 leads qualificados, as 2 oportunidades reais, os 2 transbordos reais, as 3 interações do Aroldo.

## O que implementar

### 1. Duplicidade da importação contra a base COMPLETA

1. **Service** (`src/services/leads.ts` ou `src/lib/csvImport.ts`): função que, dado o lote de contatos+empresas do arquivo, consulta a base por TODAS as correspondências (não só a página). Duas opções (escolher a mais simples):
   - Um único filtro com OR entre chaves em lotes de ~30 (limite prático de expressão do PocketBase), ou
   - Consulta por prefixo de e-mail/empresa em páginas (`perPage=200`, `skipTotal=1`) filtrando pelos contatos do arquivo.
   - NÃO usar getFullList na coleção leads (15 mil registros).
2. **Prévia da importação**: `duplicadasNaBase` passa a refletir a base inteira. A contagem e a lista de duplicadas na prévia devem bater com a base real.
3. **Prova de limite**: arquivo com contato que existe na base mas NÃO está na página 1 (ex.: lead importado antigo) deve ser sinalizado como duplicado na prévia.
4. **Desempenho**: a verificação da prévia não pode travar a tela (uma chamada ou poucas chamadas em lote; sem loop de requisição por linha).

### 2. Campo telefone no lead

1. **Migration 0023** (up/down completo): campo `telefone` (text, opcional) na coleção `leads`.
2. **Importação CSV**: mapear colunas "telefone", "celular", "phone" para o campo novo (em `mapearCabecalhos`). Se o CSV não tiver a coluna, campo fica vazio.
3. **Tela Leads**:
   - Modal Novo Lead: campo Telefone (opcional).
   - Modal Editar Lead: campo Telefone editável.
   - Listagem: sem mudança (não poluir a tabela).
4. **Modal da oportunidade** (seção Contato): mostrar Telefone com ícone próprio (Phone do lucide) quando existir, além do e-mail e das observações.
5. **Duplicidade manual** (checagem em tempo real do Novo Lead): sem mudança — continua contato+empresa.
6. **Linguagem**: label "Telefone", placeholder com exemplo em formato brasileiro ("(11) 98765-4321"). pt-BR corporativo em tudo; zero códigos internos.

## Regras obrigatórias (do CLAUDE.md — não negociável)

- **Migrations**: 0023 com down que desfaz; nunca editar migration aplicada.
- **Paginação server-side** sempre; sem getFullList em leads.
- **QA**: `skip_project_apply_changes` verde (lint + build).
- **Base real**: não apagar registros reais. Provas com dados são do Bart (insumo preparado — AP-2026-11-12-1037).
- **Linguagem**: pt-BR corporativo, maiúscula inicial, plural correto, zero códigos internos em strings de usuário.

## Definition of Done (o Bart valida isto)

- [ ] Migration 0023 aplicada (up/down OK).
- [ ] Prévia de importação sinaliza duplicado que está fora da página visível (prova com lead antigo).
- [ ] Desempenho da prévia aceitável (sem travar; poucas requisições).
- [ ] Telefone: campo novo no lead, mapeado na importação CSV, editável no Novo/Editar, visível no modal da oportunidade.
- [ ] QA do Skip verde.
- [ ] Zero códigos internos; pt-BR correto.

Ao terminar: rode o apply_changes (o webhook avisa o Bart) e NÃO conclua a task — a conclusão é do Bart após revisão e teste do owner.
