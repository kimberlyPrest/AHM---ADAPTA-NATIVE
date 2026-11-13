# Briefing — Bloco 3 da Fase 2: Permissões por papel (F2-T11)

**Data**: 13/11/2026 · **Autorizado pelo owner**: 13/11 ~13:30 · **Prioridade**: ALTA — o time de vendas (Julio e Rose) começa a usar a plataforma hoje; a base real (15.370 leads) não pode ficar exposta a qualquer usuário logado.

## Problema

Hoje **qualquer usuário autenticado faz tudo** na plataforma (leads, oportunidades, produtos, usuários, auditoria). O próprio CC já sinalizou isso como inaceitável com dados reais. Com a entrada dos testadores do time de vendas, o risco é imediato.

## Escopo

### 1. Campo de papel no usuário
- Migration: campo `papel` em `users` (options: `admin` | `vendedor`; default `vendedor`).
- O usuário existente do owner (afonso.moreira@ahmsolution.com) vira `admin`.
- Os usuários de teste recém-criados (julio@ahmsolution.com, rosemairy.freitas@ahmsolution.com) ficam `vendedor`.

### 2. Regras de acesso do vendedor
- **Pode**: ver e operar leads, oportunidades, transbordos, interações, checklist e métricas do dia a dia (criar, editar, qualificar, avançar/voltar etapa, responder checklist, registrar fechamento com motivo, arquivar/restaurar).
- **Não pode**:
  - Excluir qualquer registro (a proteção PROVA/DEMO já bloqueia; para vendedor, bloquear também a exclusão de PROVA/DEMO — exclusão física é só admin)
  - Ver coleções de auditoria (`exclusoes_auditoria`, `oportunidade_fechamentos` — leitura negada)
  - Gerenciar usuários (criar/editar/papel)
  - Editar produtos (catálogo é responsabilidade do admin)
  - Carregar/remover demonstração (só admin)
- **Admin**: tudo, como hoje (com a proteção PROVA/DEMO de registros reais mantida).

### 3. Camadas somadas
- A proteção contra exclusão do Bloco 2 (hook `protecao_exclusao.js`) continua valendo para TODOS — a regra de papel é uma camada ADICIONAL, nunca substituta.

### 4. Tela
- Em Equipe (ou equivalente): admin vê e altera o papel dos usuários. Vendedor não vê gestão de usuários.
- Sem exposição de códigos internos na UI (regra de linguagem pt-BR corporativo).

## Implementação sugerida
- API rules do PocketBase por papel (list/view/create/update/delete por coleção) + hooks de validação server-side para o que as rules não cobrem (ex.: mudança de papel só por admin).
- O papel vem do registro do usuário autenticado (não de claim editável no cliente).

## Critérios de aceite (provas que o Bart fará)
1. Login como vendedor: consegue criar/editar lead, avançar etapa, responder checklist, registrar fechamento com motivo, arquivar lead PROVA.
2. Login como vendedor: 403 em exclusão (qualquer registro), leitura de auditoria/fechamentos, gestão de usuários, edição de produto, carregar demonstração.
3. Login como admin: tudo funciona como hoje.
4. Mudança de papel por não-admin → 403; por admin → 200 e surte efeito no próximo login/request.
5. Regressão: proteção PROVA/DEMO intacta para ambos os papéis; ciclo de demonstração do admin intacto.

## Estado do projeto (contexto)
- Bloco 2 implementado e revisado; teste com usuários em andamento (Julio e Rose, prazo 1 dia útil) — **não bloqueia este bloco**; se o teste apontar ajuste no Bloco 2, o Bart manda o ponto exato e a correção entra em paralelo ou na sequência.
- Migration 0030 validada (checklist 67 itens).
- Emenda 1.3a (IA) aguarda ratificação + 10 casos — vem DEPOIS deste bloco.
