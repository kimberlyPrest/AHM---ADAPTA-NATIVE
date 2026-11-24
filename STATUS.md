# STATUS.md — estado único do projeto (fonte da verdade)

**Última atualização**: 24/11/2026 19:10 (Bart) · **Versão do working tree (Skip)**: 0.0.83, não publicada (o preview serve esta) · **Última migration**: 0040 · **Working tree**: limpo

> **PLANO DE FOCO (owner, 24/11)**: CRM é o foco; o ERP fica em MANUTENÇÃO (publicar o cálculo no servidor + corrigir o vazamento da calculadora Miami, depois congela). O Bart é o único canal de decisões: UMA mensagem por dia ao owner, tudo em formato A/B.

> Este é o ÚNICO arquivo de estado. `fase.md` = fila de tasks (estático). `changelog.md` = histórico por versão (só na conclusão). `.adapta-cliente/estado-atual.md` = APOSENTADO (aponta para cá). `CLAUDE.md` = convenções técnicas (não cita número de versão/migration — aponta para cá). `CARTA-BART.md` = papel e regras do Bart (v1.2 publicada; aprovação final do owner pendente). OBS: a publicação pelo chat apagou a formatação (títulos, negrito, tabelas, bloco de código) da carta e das notas — a versão formatada sobe quando o acesso ao GitHub sair; não republicar por cima.

## Em curso

| O quê | Quem | Estado | Próximo passo |
|---|---|---|---|
| **Bloco R1** — bugs da Rose + datas reais | CC (feito) / Bart (provado) | **PRONTO TECNICAMENTE — aguardando teste humano da Rose e do Julio** (e-mail de convite enviado hoje; Rose refaz o caso real com as datas reais; conclusão formal só depois, pela carta seção 8) | Teste da Rose e do Julio → "pode concluir" do owner |
| Segurança da equipe — 0.0.79/0.0.83 | CC (feito) / Bart (provado) | **PRONTO PARA O "PODE CONCLUIR"** — B1 corrigido (0.0.82, GitHub bd8a881) + débito de tela corrigido (0.0.83, GitHub 9f2da1e): bloqueado ≡ anônimo; aba aberta → aviso "Sua sessão foi encerrada" → login. Débito restante: erro de escrita genérico (R8 cobre no passo 2 do contrato). Contas: PROVA Vendedor (Bart) ATIVA; Rose Teste 1/2 bloqueadas ("Conta de teste") — owner nunca tinha feito (erro de redação; não há outro banco) | "pode concluir" do owner |
| Contrato comum CRM + ERP (v1) | CC escreveu / Bart confirmou | **CONFIRMADO PELO BART (1 linha, 24/11 19:00)** — v1 no Skip (0.0.83) com os 4 ajustes aplicados corretamente + regra de moeda/entidade (uma fonte só: CRM deriva, ERP consome) + autenticação por credencial de serviço nos segredos de cada plataforma | Aprovação do owner → versão-mestre do padrão R1–R10 |
| **ERP — MANUTENÇÃO** (frente paralela) | CC do ERP | Plano do owner: publicar o cálculo no servidor + corrigir o vazamento da calculadora Miami, depois CONGELA | CC do ERP entrega as 2 correções → congela até nova ordem |
| Roteamento por família (migration 0036) | CC (implementa) | AGUARDANDO OWNER | Owner preenche as 4 planilhas (roteamento-ia-1..4) e autoriza |
| F0.6 — app Entra ID (Mail.Read/Send caixa CEO, Calendars, transcrições; Access Policy → "Comercial – IA") | TI da AHM | AGUARDANDO TI | Owner encaminha especificação; Bart testa credenciais no dia da entrega |
| APIs RD Station + Envya | Owner → especialista | SOLICITADO | E-mail pronto (artifacts/emails-acesso-api-rd-envya-m365.md) |
| Perguntas ao suporte Skip ($ai.chat: modelo, BYOK, limites, LGPD) | Owner/CC → suporte | EM ABERTO | Resposta decide IA 100% Skip vs serviço à parte |
| Cadastro do Paulo | Owner passa nome+e-mail → Bart cria via API | AGUARDANDO OWNER | — |

## Fila próxima (ordem do owner, 24/11)

1. **Catálogo completo de produtos** — aguardando a lista da Rose (C1 do feedback)
2. **Bloco R2** (após o teste da Rose): P1 observação livre por fase · P2 proposta (PDF) ligada à oportunidade · P4 ocorrência na implementação — tudo já no padrão R1–R10
3. **Entrada em uso pelo time**
4. **Migration 0036** (roteamento por família) — quando o owner entregar as 4 planilhas

## Bloqueado (por quem)

| O quê | Bloqueio | Desde |
|---|---|---|
| Importação Pipedrive (Bloco D pronto, 0.0.73) | OWNER suspendeu — histórico não confiável | 14/11 |
| F1 completo (webhook RD, Envya, geração, envio) | APIs + especialistas + vídeos + F0.6 | 14/11 |

## Pronto (aprovado e verificado)

| Entrega | Versão | Provas | Data |
|---|---|---|---|
| Segurança da equipe (bloqueio, sessões, redefinição) — provas Bart | 0.0.79–0.0.83 | 22 provas + B1 re-provado + débito de tela re-provado | 24/11 |
| Teste com usuários — retorno da Rose analisado | — | análise 06_notas/analise-feedback-rose-2026-11-24.md | 24/11 |
| Padrão operável por agentes R1–R10 RATIFICADO (emenda 1.4a) | — | 06_notas/proposta-padrao-operavel-por-agentes.md | 24/11 |
| F0.1–F0.5 IA de vendas (base conhecimento, roteamento, janelas, auditoria, modo rascunho) | 0.0.74 | Bart 31/31 API + tela | 14/11 |
| Bloco D — importação Pipedrive (rota/tela prontas, nada importado) | 0.0.73 | Bart a–f | 14/11 |
| Bloco C — Desempenho/Minha mesa/motivos de perda | 0.0.72 | Bart (a)–(f) | 13/11 |
| Blocos A+B — segurança/estabilidade + telas grandes | 0.0.68–0.0.69 | Bart 32/32 | 13/11 |
| Bloco 3 — permissões por papel | 0.0.67 | Bart 23/24 | 13/11 |
| Bloco 2 — proteção/checklist/fechamento/taxas | 0.0.62–0.0.64 | Bart + owner | 13/11 |
| Bloco 1 — entrada/pipeline/2 trilhas/lote | 0.0.60–0.0.61 | Bart 8 provas | 13/11 |
| Fase 1 completa (23/23) | ≤0.0.59 | owner | 13/11 |

## Decisões do owner pendentes

1. Aprovação final da CARTA-BART v1.2
2. Kimberly como admin — confirmar
3. Casos reais da Rose (Red Zone, Veronica) — ficam reais ou são marcados/arquivados
4. Autorizar a 0036 (roteamento por família) — após planilhas
5. Destino do "SLA meta < 30 min" (configuração ou remoção)
6. Ativar "Outro (especificar)" nos motivos de perda
7. Par duplicado do Aroldo — qual manter
8. **Aprovar o Contrato comum CRM + ERP v1** (confirmado pelo Bart 24/11 19:00)

## Riscos ativos

| Risco | Gravidade | Mitigação |
|---|---|---|
| Senha do admin exposta em versões antigas + GitHub | ALTA | Owner trocar (pendente desde 13/11) |
| Senha de teste do Julio (Teste@CRM2026) ainda ativa até o Julio redefinir pelo link | ALTA | Link enviado pelo owner (18:00); confirmar redefinição |
| Regras de prova do Bart: NÃO usar contas de pessoas reais (Julio, Rose) — usar PROVA Vendedor (Bart), criada e ativa na base | — | Owner, 24/11 — regra permanente |
| **Vendedor enxerga TODAS as leads** (regra da coleção desde a Fase 1: "qualquer usuário logado"; "Minha mesa" filtra por responsável, a tela de Leads não) — nota de desenho, decisão de negócio do owner para a entrada em uso | MÉDIA | Registrar decisão antes da entrada em uso pelo time |
| Gráficos do Desempenho nascem vazios (só 2 ops reais) | MÉDIA | Importação suspensa — aceito pelo owner |
| Hooks after-success sem e.next() (warning recorrente) | BAIXA | Monitorado nos logs; último 12/11 12:53 |
