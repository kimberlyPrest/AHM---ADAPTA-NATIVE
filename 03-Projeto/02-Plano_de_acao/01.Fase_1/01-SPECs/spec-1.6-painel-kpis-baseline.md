# SPEC-1-006 — Painel mínimo de K1/K2/K3 e baseline

**Fase:** 1  
**Status:** planejada  
**Dono:** Consultora / AHM  
**Origem no escopo:** R-03, R-04, R-05 / C-06  
**Degrau da solução:** construção mínima — métricas precisam existir antes das integrações para provar valor.

## Contexto e decisões fechadas

- **Estado atual:** os KPIs do briefing existem, mas a Fase 1 pode operar com dados manuais/amostras.
- **Estado desejado:** painel mostra baseline e cálculo inicial de K1, K2 e K3 usando leads e interações registradas no sistema.
- **Decisões já fechadas:** fonte manual é válida na Fase 1; integração automatizada não é requisito.
- **Bloqueios:** nenhum.

## Resultado observável

Um painel mínimo mostra K1, K2 e K3 com período, fonte, numerador, denominador e indicação de dados insuficientes quando aplicável.

## Limites e dependências

- **Inclui:** cálculo de K1, K2, K3, filtros simples por período e fonte, estado sem dados.
- **Fora de escopo:** BI avançado, integração CRM, dashboard executivo completo.
- **Entradas e pré-condições:** leads, qualificações e interações registradas.
- **Saídas/artefatos:** painel/relatório de baseline.
- **Dependências e responsáveis:** AHM valida interpretação dos KPIs.
- **Atores e permissões mínimas:** consultora/comercial visualiza.
- **Superfícies/arquivos/configurações afetadas:** módulo de métricas.
- **Risco e plano B:** baixo volume; mostrar insuficiência em vez de sucesso falso.
- **Rollback ou reversão:** métricas recalculam a partir dos eventos.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Leads/qualificações/interações | Sistema Fase 1 | status, datas, interações, vendedor/responsável, período | usuário interno | cálculo determinístico | dado ausente aparece como insuficiente |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.16 | lead qualificado automaticamente | entra no numerador do K1 | transbordo/pendente não entra | AHM.pdf |
| RN-1.17 | lead recebido no recorte piloto | entra no denominador do K1 | fora do recorte não entra | escopo definitivo |
| RN-1.18 | interação humana registrada até proposta | compõe média do K2 | interação automática não conta | AHM.pdf |
| RN-1.19 | leads processados por vendedor | compõe K3 | sem responsável fica em pendência de dados | AHM.pdf |

## Fluxo e regras

1. Sistema lê leads, qualificações e interações.
2. Calcula K1, K2 e K3 para o período.
3. Mostra baseline e lacunas.
4. Usuário exporta ou registra evidência.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | dados completos | painel calcula KPIs | não aplicável |
| Limite | sem dados suficientes | painel mostra insuficiente | orientar dados faltantes |
| Falha | divisão por zero | não quebrar; mostrar zero/insuficiente | mensagem clara |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** SPECs 1.2, 1.3, 1.7.
2. **Alterar somente:** painel/cálculo de métricas.
3. **Não alterar:** regras de qualificação já aceitas.
4. **Executar nesta ordem:** definir eventos, cálculo, UI/relatório, estados vazios, testes.
5. **Parar e pedir validação quando:** fórmula de KPI precisar mudar.
6. **Estado válido ao parar:** leads e oportunidades seguem funcionando.

## Checklist de execução

- [ ] K1 calcula numerador/denominador.
- [ ] K2 calcula média de interações humanas até proposta.
- [ ] K3 calcula capacidade por vendedor/responsável.
- [ ] Estado sem dados é claro.
- [ ] Período/fonte aparecem no painel.

## Critérios de aceite

- [ ] **CA-1-016:** painel mostra K1 com numerador e denominador.
- [ ] **CA-1-017:** painel mostra K2 com média de interações humanas.
- [ ] **CA-1-018:** painel mostra K3 ou insuficiência de dados sem quebrar.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | abrir painel sem cálculo | acessar métricas | KPIs indisponíveis ou vazios | captura/log |
| GREEN | carregar massa mínima | 10 leads, 7 qualificados, interações registradas | K1 70%, K2 calculado, K3 exibido | captura/log |
| REFACTOR/REGRESSÃO | período sem leads | filtrar período vazio | sem erro; mostra dados insuficientes | captura/log |

**Dados/fixtures:** massa mínima com 10 leads do recorte, 7 qualificados, responsáveis e interações.  
**Caminhos de erro obrigatórios:** período vazio, responsável ausente, zero denominador.  
**Evidência exigida:** captura/log do painel com fórmulas visíveis.

## Handoff e operação

- **Como demonstrar:** carregar fixtures e mostrar os três KPIs.
- **Como operar depois:** consultora/AHM revisa baseline semanalmente.
- **Como monitorar:** dados insuficientes e distribuição por status.
- **Pendência conhecida:** automação da coleta fica para Fase 2.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Critério | Recorte da prova | Evidência esperada | Pré-condições | Status |
|---|---|---|---|---|---|---|---|---|
| F1-T16 | Criar eventos/base de cálculo de métricas | Ethos | SPEC 1.6 | K1, K2 e K3 leem dados locais | CA-1-016, CA-1-017, CA-1-018 | log/captura | F1-T07, F1-T19 | ☐ |
| F1-T17 | Criar painel mínimo com filtros e estado sem dados | Ethos | SPEC 1.6 | Painel mostra KPIs ou insuficiência sem erro | CA-1-016, CA-1-017, CA-1-018 | captura | F1-T16 | ☐ |
| F1-T18 | Criar fixture de baseline com 10 leads | Ethos | SPEC 1.6 | Fixture demonstra K1 70% e K2/K3 calculáveis | TDD GREEN SPEC 1.6 | captura | F1-T17 | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
