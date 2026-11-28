# STATUS.md — estado único do projeto (fonte da verdade)

**Última atualização**: 28/11/2026 13:35 (Bart) — análise da recompra publicada · **Versão do working tree (Skip)**: 0.0.83, não publicada (o preview serve esta) · **Última migration**: 0040 · **Working tree**: limpo

> **PLANO DE FOCO (owner, 24/11)**: CRM é o foco; o ERP fica em MANUTENÇÃO (publicar o cálculo no servidor + corrigir o vazamento da calculadora Miami, depois congela). O Bart é o único canal de decisões: UMA mensagem por dia ao owner, tudo em formato A/B.

> **NORTE DA EMPRESA (owner, 26/11)**: as Pedras Fundamentais (00_cultura/pedras-fundamentais.md, push pendente do CC) são leitura obrigatória antes de propor ou revisar; desempatam opções equivalentes; conflito com Pedra = apontar com gravidade; atitude esperada = antecipação ativa. Somos um time: owner, Bart e CC.

> Este é o ÚNICO arquivo de estado. `fase.md` = fila de tasks (estático). `changelog.md` = histórico por versão (só na conclusão). `.adapta-cliente/estado-atual.md` = APOSENTADO (aponta para cá). `CLAUDE.md` = convenções técnicas (não cita número de versão/migration — aponta para cá). `CARTA-BART.md` = papel e regras do Bart (v1.2 publicada; aprovação final do owner pendente).

## Em curso

| O quê | Quem | Estado | Próximo passo |
|---|---|---|---|
| Transição Cilene (sai 30/11) — faturamento vira agente | CC (roteiro) | **URGENTE**: faturamento (caixa) não pode parar a partir de terça 01/12; reunião segunda cedo (2–3h) com Cilene, Rose e Mario, GRAVADA no Fyxer; roteiro do CC em 06_notas/transicao-atividades-assistente-administrativa.md (tabela A/HA/H, passagem de bastão, acessos c/ troca de senhas nunca por e-mail/Bart/CC, caixa dela redirecionada p/ Rose via Atual-IT) | Reunião gravada → material base dos agentes de faturamento e compras |
| Recompra do cíclico (adendo do owner) | CC / Bart / Rose | Desenho v0 APROVADO (owner respondeu as 4 perguntas: contato proativo = 1ª automação; preço 1 ano = candidato a automático c/ conferente avisando margem por câmbio; consumo mensal = pergunta obrigatória do diagnóstico; ideia de programa anual). **Material da idealização EXTRAÍDO E ANALISADO pelo Bart** (06_notas/analise-material-idealizacao-recompra.md, GitHub 463edf7): o que já está definido (ciclo 45–120d por modelo, campos, A5, fase do mapa) + **8 gaps a gravar com a Rose na reunião de segunda** | Reunião 30/11 (Cilene, Rose, Mario, gravada) fecha os gaps → v1 do desenho |
| Push dos 16+ commits locais (Pedras, estrutura v1, análise do mapa atualizada, CLAUDE.md, decisões 28/11, fluxo de recompra) | CC (na máquina do owner) | **DECISÃO DO OWNER (28/11)**: Kimberly FICA no projeto (design e verificações de segurança); SEM migração por enquanto. Caminho do push: pedir escrita à Kimberly p/ a conta afonsoahm-netizen, OU transferir o repo p/ ahmsolution-ahm com ela como colaboradora | Owner pede acesso à Kimberly → CC faz o push → Bart lê as Pedras e incorpora à revisão |
| Análise do mapa de processos + proposta de agentes (CC, 25/11) | CC escreveu / Bart revisou | **AJUSTES DA REVISÃO APLICADOS PELO CC**. Seção 8 respondida (26/11): compras nacionais + prospecção (Apollo) têm procedimento v0; limites de escala FICAM VAZIOS por decisão do owner (inadimplência rara — tudo escala); marketing: sem agência — **Spalla** (consultoria) coordena os fornecedores | Fundação (passo 0) vira briefing (abaixo) |
| Fundação (passo 0) + piloto de Onboarding (ShockWatch Label) | CC (análise/briefing primeiro) | **AUTORIZADO PELO OWNER (28/11), no protocolo**: CC traz a análise/briefing e PARA no portão ("Pode implementar?") | Briefing do passo 0 (SLA por espera + baseline) e do piloto de onboarding |
| Matriz de autonomia (automático · rascunho · sempre humano, c/ regra de promoção) | CC | **APROVADA POR ENQUANTO (28/11)** — 06_notas/prontidao-automacao-ponta-a-ponta.md, seção 2.5 | Registra no padrão dos agentes |
| **Bloco R1** — bugs da Rose + datas reais | CC (feito) / Bart (provado) | **PRONTO TECNICAMENTE — aguardando teste humano da Rose e do Julio** (owner vai encaminhar um e-mail deles — provável retorno do teste) | Teste da Rose e do Julio → confirmação formal |
| Segurança da equipe — 0.0.79/0.0.83 | CC (feito) / Bart (provado) | **"PODE CONCLUIR" RECEBIDO DO OWNER (28/11)** — bloco concluído | changelog na conclusão (CC) |
| Contrato comum CRM + ERP (v1) | CC escreveu / Bart confirmou / owner aprovou | **APROVADO PELO OWNER (28/11) — VERSÃO-MESTRE do padrão R1–R10** para CRM, ERP e Chief of Staff | Passo 2 do contrato (campos de auditoria + ocorrido_em) quando a fila abrir |
| F0.6 — app Entra ID (Mail.Read/Send caixa CEO, Calendars, transcrições; Access Policy → "Comercial – IA") | TI da AHM | AGUARDANDO TI. **Decisão do owner (28/11)**: usar **vendas@** como caixa comercial nova e MANTER o grupo "Comercial" existente; o OWNER envia a resposta à Atual-IT; quando a Atual-IT voltar, conferir se a resposta está correta ANTES do primeiro agente | Owner envia a resposta (rascunho v3 pronto) |
| **ERP — MANUTENÇÃO** (frente paralela) | CC do ERP | Plano do owner: publicar o cálculo no servidor + corrigir o vazamento da calculadora Miami, depois CONGELA | CC do ERP entrega as 2 correções → congela até nova ordem |
| Roteamento por família (migration 0036) | CC (implementa) | AGUARDANDO OWNER | Owner preenche as 4 planilhas (roteamento-ia-1..4) e autoriza |
| APIs RD Station + Envya | Owner → especialista | SOLICITADO | E-mail pronto (artifacts/emails-acesso-api-rd-envya-m365.md) |
| Perguntas ao suporte Skip ($ai.chat: modelo, BYOK, limites, LGPD) | Owner/CC → suporte | EM ABERTO | Resposta decide IA 100% Skip vs serviço à parte |
| Cadastro do Paulo | Owner passa nome+e-mail → Bart cria via API | AGUARDANDO OWNER | — |

## Fila próxima (ordem do owner, 24/11 + análise do mapa 26/11)

1. **Fundação (passo 0 da análise do mapa)**: casos com dono, etapa, prazo e "parado há N dias" + painel único — briefing do CC no portão; inclui SLA por tipo de espera e baseline de medição
2. **Piloto de Onboarding**: família Impacto/tombamento (ShockWatch Label); formato do pré-mortem — CRM como registro do caso + e-mails em rascunho até a F0.6
3. **Catálogo completo de produtos** — aguardando a lista da Rose (C1 do feedback)
4. **Bloco R2** (após o teste da Rose): P1 observação livre por fase · P2 proposta (PDF) ligada à oportunidade · P4 ocorrência na implementação — tudo já no padrão R1–R10
5. **Entrada em uso pelo time** (vendedor enxerga todas as leads: MANTER por enquanto — decisão 28/11)
6. **Migration 0036** (roteamento por família) — quando o owner entregar as 4 planilhas

## Bloqueado (por quem)

| O quê | Bloqueio | Desde |
|---|---|---|
| Importação Pipedrive (Bloco D pronto, 0.0.73) | OWNER suspendeu — histórico não confiável | 14/11 |
| F1 completo (webhook RD, Envya, geração, envio) | APIs + especialistas + vídeos + F0.6 | 14/11 |
| Push dos commits locais | Conta da máquina sem escrita no repo — owner pede escrita à Kimberly ou transfere o repo (decisão 28/11) | 26/11 |

## Pronto (aprovado e verificado)

| Entrega | Versão | Provas | Data |
|---|---|---|---|
| Segurança da equipe (bloqueio, sessões, redefinição) — CONCLUÍDA ("pode concluir" 28/11) | 0.0.79–0.0.83 | 22 provas + B1 re-provado + débito de tela re-provado | 24–28/11 |
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
3. Autorizar a 0036 (roteamento por família) — após planilhas
4. Destino do "SLA meta < 30 min" (configuração ou remoção)
5. Ativar "Outro (especificar)" nos motivos de perda
6. Par duplicado do Aroldo — qual manter

## Resolvidas pelo owner (28/11)

- Contrato comum CRM + ERP v1: **APROVADO** (versão-mestre)
- Segurança da equipe (0.0.79–0.0.83): **"pode concluir"**
- Kimberly: **FICA** no projeto (design e verificações de segurança) — desbloqueio confirmado na base (já estava ativa ao conferir 07:00)
- Vendedor enxerga todas as leads: **MANTER, por enquanto**
- Matriz de autonomia (seção 2.5 de prontidao-automacao-ponta-a-ponta.md): **aprovada por enquanto**
- Atual-IT: **vendas@** como caixa comercial nova, grupo "Comercial" mantido; owner envia a resposta
- Recompra do cíclico: **4 perguntas respondidas, desenho v0 aprovado**; Rose fica como supervisora dos agentes de Adm de Vendas; Cilene sai 30/11 (faturamento vira agente)

## Riscos ativos

| Risco | Gravidade | Mitigação |
|---|---|---|
| **Cilene sai 30/11 e o faturamento (caixa) não pode parar a partir de 01/12** | ALTA | Reunião gravada segunda cedo (Cilene, Rose, Mario); roteiro do CC; base dos agentes de faturamento/compras |
| Senha do admin exposta em versões antigas + GitHub | ALTA | Owner trocar (pendente desde 13/11) |
| Senha de teste do Julio (Teste@CRM2026) ainda ativa até o Julio redefinir pelo link | ALTA | Link enviado pelo owner (24/11 18:00); confirmar redefinição |
| Regras de prova do Bart: NÃO usar contas de pessoas reais (Julio, Rose) — usar PROVA Vendedor (Bart), criada e ativa na base | — | Owner, 24/11 — regra permanente |
| Medição de LinkedIn: 403 no conector (falta escopo r_organization_social); comentários não legíveis via API | MÉDIA | Fallback manual (owner copia do painel); dimensionar antes de prometer medição automática |
| Gráficos do Desempenho nascem vazios (só 2 ops reais) | MÉDIA | Importação suspensa — aceito pelo owner |
| Hooks after-success sem e.next() (warning recorrente) | BAIXA | Monitorado nos logs; último 12/11 12:53 |
