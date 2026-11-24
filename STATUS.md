# STATUS.md — estado único do projeto (fonte da verdade)

**Última atualização**: 24/11/2026 13:00 (Bart) · **Versão em produção**: 0.0.76 · **Última migration**: 0037 · **Working tree**: limpo

> Este é o ÚNICO arquivo de estado. `fase.md` = fila de tasks (estático). `changelog.md` = histórico por versão (só na conclusão). `.adapta-cliente/estado-atual.md` = APOSENTADO (aponta para cá). `CLAUDE.md` = convenções técnicas (não cita número de versão/migration — aponta para cá). `CARTA-BART.md` = papel e regras do Bart (v1.2 publicada; aprovação final do owner pendente).

## Em curso

| O quê | Quem | Estado | Próximo passo |
|---|---|---|---|
| Bloco R1 (bugs da Rose) — 0.0.76 | CC | REVISADO PELO BART: APROVADO COM A1 (etapa_ocorrido_em persiste na criação e contamina updates — correção de 1 linha no hook create) | CC corrige A1 → Bart re-prova (2 provas) → "pode concluir" |
| Roteamento por família (migration 0036) | CC (implementa) | AGUARDANDO OWNER | Owner preenche as 4 planilhas (roteamento-ia-1..4) e autoriza |
| F0.6 — app Entra ID (Mail.Read/Send caixa CEO, Calendars, transcrições; Access Policy → "Comercial – IA") | TI da AHM | AGUARDANDO TI | Owner encaminha especificação; Bart testa credenciais no dia da entrega |
| APIs RD Station + Envya | Owner → especialista | SOLICITADO | E-mail pronto (artifacts/emails-acesso-api-rd-envya-m365.md) |
| Perguntas ao suporte Skip ($ai.chat: modelo, BYOK, limites, LGPD) | Owner/CC → suporte | EM ABERTO | Resposta decide IA 100% Skip vs serviço à parte |
| Cadastro do Paulo | Owner passa nome+e-mail → Bart cria via API | AGUARDANDO OWNER | — |

## Bloqueado (por quem)

| O quê | Bloqueio | Desde |
|---|---|---|
| Importação Pipedrive (Bloco D pronto, 0.0.73) | OWNER suspendeu — histórico não confiável | 14/11 |
| F1 completo (webhook RD, Envya, geração, envio) | APIs + especialistas + vídeos + F0.6 | 14/11 |

## Pronto (aprovado e verificado)

| Entrega | Versão | Provas | Data |
|---|---|---|---|
| Revisão do Bloco R1 (B1–B4 da Rose provados; A1 pendente) | 0.0.76 | Bart: 30 provas API + tela; relatório 06_notas/revisao-bloco-r1-provas-bart.md | 24/11 |
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
2. Casos reais da Rose (Red Zone, Veronica) — ficam reais ou são marcados/arquivados
3. Trocar senhas de teste de Julio e Rose (enviadas por e-mail em 13/11)
4. Autorizar a 0036 (roteamento por família) — após planilhas
5. Destino do "SLA meta < 30 min" (configuração ou remoção)
6. Ativar "Outro (especificar)" nos motivos de perda
7. Par duplicado do Aroldo — qual manter

## Riscos ativos

| Risco | Gravidade | Mitigação |
|---|---|---|
| Senha do admin exposta em versões antigas + GitHub | ALTA | Owner trocar (pendente desde 13/11) |
| Gráficos do Desempenho nascem vazios (só 2 ops reais) | MÉDIA | Importação suspensa — aceito pelo owner |
| Hooks after-success sem e.next() (warning recorrente) | BAIXA | Monitorado nos logs; último 12/11 12:53 |
