# STATUS.md — estado único do projeto (fonte da verdade)

**Última atualização**: 14/11/2026 21:30 (Bart) · **Versão em produção**: 0.0.74 · **Última migration**: 0035 · **Working tree**: limpo

> Este é o ÚNICO arquivo de estado. `fase.md` = fila de tasks (estático). `changelog.md` = histórico por versão (só na conclusão). `.adapta-cliente/estado-atual.md` = APOSENTADO (aponta para cá). `CLAUDE.md` = convenções técnicas (não é estado).

## Em curso

| O quê | Quem | Estado | Próximo passo |
|---|---|---|---|
| Roteamento por família (migration 0036) | CC (implementa) | AGUARDANDO OWNER | Owner preenche as 4 planilhas (roteamento-ia-1..4) e autoriza |
| F0.6 — app Entra ID (Mail.Read/Send caixa CEO, Calendars, transcrições; Access Policy → "Comercial – IA") | TI da AHM | AGUARDANDO TI | Owner encaminha especificação; Bart testa credenciais no dia da entrega |
| Teste com usuários (Bloco 2 + telas novas) | Julio e Rose | EM ANDAMENTO | Fichas de resposta pendentes |
| APIs RD Station + Envya | Owner → especialista | SOLICITADO | E-mail pronto (artifacts/emails-acesso-api-rd-envya-m365.md) |
| Perguntas ao suporte Skip ($ai.chat: modelo, BYOK, limites, LGPD) | Owner/CC → suporte | EM ABERTO | Resposta decide IA 100% Skip vs serviço à parte |

## Bloqueado (por quem)

| O quê | Bloqueio | Desde |
|---|---|---|
| Importação Pipedrive (Bloco D pronto, 0.0.73) | OWNER suspendeu — histórico não confiável | 14/11 |
| Cadastro do Paulo | Owner passar nome+e-mail (Bart cria via API) | 14/11 |
| F1 completo (webhook RD, Envya, geração, envio) | APIs + especialistas + vídeos + F0.6 | 14/11 |

## Pronto (aprovado e verificado)

| Entrega | Versão | Provas | Data |
|---|---|---|---|
| F0.1–F0.5 IA de vendas (base conhecimento, roteamento, janelas, auditoria, modo rascunho) | 0.0.74 | Bart 31/31 API + tela | 14/11 |
| Bloco D — importação Pipedrive (rota/tela prontas, nada importado) | 0.0.73 | Bart a–f | 14/11 |
| Bloco C — Desempenho/Minha mesa/motivos de perda | 0.0.72 | Bart (a)–(f) | 13/11 |
| Blocos A+B — segurança/estabilidade + telas grandes | 0.0.68–0.0.69 | Bart 32/32 | 13/11 |
| Bloco 3 — permissões por papel | 0.0.67 | Bart 23/24 | 13/11 |
| Bloco 2 — proteção/checklist/fechamento/taxas | 0.0.62–0.0.64 | Bart + owner | 13/11 |
| Bloco 1 — entrada/pipeline/2 trilhas/lote | 0.0.60–0.0.61 | Bart 8 provas | 13/11 |
| Fase 1 completa (23/23) | ≤0.0.59 | owner | 13/11 |

## Decisões do owner pendentes

1. Autorizar a 0036 (roteamento por família) — após planilhas
2. Destino do "SLA meta < 30 min" (configuração ou remoção)
3. Ativar "Outro (especificar)" nos motivos de perda
4. Par duplicado do Aroldo — qual manter
5. Aposentar arquivos de estado duplicados (carta do Bart v0) — **aguardando aprovação**

## Riscos ativos

| Risco | Gravidade | Mitigação |
|---|---|---|
| Senha do admin exposta em versões antigas + GitHub | ALTA | Owner trocar (pendente desde 13/11) |
| Gráficos do Desempenho nascem vazios (só 2 ops reais) | MÉDIA | Importação suspensa — aceito pelo owner |
| Hooks after-success sem e.next() (warning recorrente) | BAIXA | Monitorado nos logs; último 12/11 12:53 |
