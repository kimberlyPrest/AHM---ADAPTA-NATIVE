# Revisão única — Contrato comum CRM + ERP (v0) · Bart · 24/11/2026 18:20

**Estado conferido em 24/11 18:15: CRM versão 0.0.80, migration 0040, working tree limpo; contrato ainda não está no repo nem no Skip (revisão feita sobre o texto colado pelo owner).**

**Resultado: APROVADO COM 4 AJUSTES (nenhum estrutural) — o desenho está correto e consistente com o que existe nos dois lados.**

## Checklist contra o que já existe (verificado no código/base real)

| Afirmação do contrato | Conferência |
|---|---|
| CRM dono de cliente/contato/oportunidade/produto comercial (opção A) | ✅ consistente com o mapa aprovado |
| Moeda e entidade vendedora derivadas no CRM | ✅ `on_oportunidade_pipeline.js` deriva BRL/AHM Brasil e USD/AHM Solution Americas do `ambiente_fechamento` — confirmado no código |
| Ponto 9.2: "empresa" do lead é texto hoje | ✅ confirmado na base (texto livre, ex.: "Stellantis (Mopar)"); não existe `crm_cliente_id` no lead — o passo 4 é necessário e bem colocado |
| R9 (eventos) não existe no CRM | ✅ confirmado — nenhum evento de saída; o passo 5 cria |
| R6 (ocorrido_em) já existe no CRM | ✅ Bloco R1/0.0.78 — os nomes batem (`ocorrido_em`, `data_informada`, created separado) |
| R8 (códigos estáveis) | 🟡 parcial no CRM — `agenda_indisponivel` e `link_enviado` já seguem o formato; a maioria dos erros ainda é só mensagem (sem código) — o passo 2 do contrato fecha isso |

## Ajustes pedidos (para a v1 — o CC aplica; eu não reescrevo)

1. **(conteúdo) Seção 3 — `motivo` obrigatório "quando o agente altera" precisa de exceção explícita para registros PROVA e para ações de leitura.** Como está, toda escrita de agente exige `motivo` — o ciclo de provas do Bart (que vai operar como agente no padrão) criaria registros PROVA sem motivo e falharia. Sugestão: "motivo é obrigatório quando o agente altera **dado real**; registros de prova (prefixo PROVA) gravam o motivo do teste". Sem isso, o contrato trava a própria esteira de revisão.
2. **(conteúdo) Seção 5 — a integração inicial precisa dizer quem grava o `erp_proposta_id` no CRM e com que permissão.** O CRM é dono da oportunidade — o ERP não pode escrever nela; mas o contrato diz que "a oportunidade no CRM recebe link, valor e PDF". Isso exige uma rota no CRM (ex.: `POST /backend/v1/oportunidades/{id}/proposta-erp`) autenticada como **agente do ERP**, validando id + origem — ou o ERP escreve via evento e o CRM consome. Como está, a regra 1 ("só o dono altera") é contradita pela integração. É o mesmo padrão do B1 do R1: quem escreve, escreve pela porta do dono.
3. **(menor) Seção 2 — falta o destino do papel "operador" do ERP.** O ERP está criando papel "operador" (passo 1, autorizado) — o contrato define `tipo = agente` mas não diz se "operador" é humano com permissão mínima ou agente. Uma linha resolve: "operador = humano com permissão mínima de operação (não é agente)".
4. **(menor) Seção 3 — `chave_idempotencia` precisa de política de retenção.** Se toda escrita de agente grava a chave, a tabela cresce sem limpeza. Sugestão: "a chave é única por tabela e pode ser apagada após N dias (definido por sistema); a idempotência vale na janela de retenção".

## O que está bom (registro para o owner)

- A divisão "o que é comum é o combinado, não o código" é a leitura correta para duas stacks diferentes — e o formato (tabelas curtas, nomes iguais nos dois lados) é verificável na revisão.
- A regra "herdados sem id ficam 'a conferir', nunca adivinhados" é exatamente a lição do incidente de duplicados (AP-2026-11-13-2136) e da importação suspensa do Pipedrive — consistente com a história do projeto.
- A ordem de adoção respeita o plano de foco: ERP só nos passos 1–2 (manutenção) e o CRM puxa a frente (passos 2–6).
- Os 3 pontos em aberto são reais e bem escolhidos (verifiquei o 9.2 na base: `empresa` é texto livre mesmo).

## Resposta técnica aos pontos em aberto (seção 9)

- **9.1 Autenticação CRM↔ERP**: no lado do CRM (Skip/PocketBase), o caminho natural é uma credencial de serviço por sistema guardada em secret do projeto (o Skip tem `skip_cloud_set_secret`) + rota autenticada por ela — não por login de usuário. Confirmar o equivalente no Supabase (service role key). O contrato pode registrar a intenção e deixar o mecanismo por sistema.
- **9.2 Cadastro mestre de cliente**: confirmado — `empresa` do lead é texto livre; o passo 4 (coleção `clientes` + migração com conferência humana) é o caminho. Nota: a deduplicação por empresa usada na importação (chave nome+empresa) continua válida como fallback enquanto o id não existe.
- **9.3 Moeda/entidade**: as regras do CRM são as do `ambiente_fechamento` (Venda/Locação em reais → BRL + AHM Brasil; Exportação (EUA) → USD + AHM Solution Americas). O ERP deve consumir o valor derivado pelo CRM (via evento) e nunca re-derivar — uma fonte só. Sugestão: registrar isso no contrato como regra explícita.

## Próximo passo

CC aplica os 4 ajustes → publica a v1 no repo (junto com `avaliacao-erp-padrao-r1-r10.md`, citado mas ausente) → eu confirmo em 1 linha → owner aprova. Depois de aprovado, vira a versão-mestre e o passo 2 (campos de auditoria no CRM) pode ser briefingado.
