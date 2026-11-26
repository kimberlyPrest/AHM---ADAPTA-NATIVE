# Revisão única — Análise do mapa de processos e proposta de agentes (CC, 25/11) · Bart · 26/11/2026 13:30

**Base da revisão:** texto integral fornecido pelo owner (o arquivo ainda não está no repo/Skip — push do CC pendente, junto com pedras-fundamentais.md e estrutura-ahm-funcoes-agentes.md). O `.vsdx` original está na máquina do owner; reviso contra a transcrição do CC, não contra o arquivo. Referências às Pedras (ex.: "Pedra 2 — experiência e movimento de fãs") serão conferidas quando `00_cultura/pedras-fundamentais.md` chegar.

**Resultado: APROVADO COM AJUSTES (nenhum bloqueante) — a análise é forte e as 4 causas raiz batem com o que os mapas consolidados em artifacts/ já mostravam.**

## O que está bom (registro para o owner)

- As 4 causas raiz (esperas sem relógio, caso sem dono/estado único, redigitação, decisões implícitas) são a mesma conclusão a que cheguei na consolidação dos dois mapas — convergência independente é o melhor sinal de que a causa está certa.
- "Colocar agentes sem resolver 1 e 2 automatiza o caos" é a frase mais importante do documento — e a ordem recomendada (fundação primeiro) respeita isso.
- A classificação das decisões (determinística/probabilística/escalada) é exatamente o que o padrão R1–R10 precisa para virar regra de servidor; os limites pendentes (margem, desconto, numerário, inadimplência) ficarem vazios = tudo escala é o comportamento seguro correto (R4: padrão rascunho).
- O relatório de gargalos mensal como padrão de todos os agentes conecta a operação à evolução dos sistemas — é o mecanismo que faz "ferramentas próprias nascem dos gargalos" deixar de ser frase e virar fila priorizada.
- Onboarding com fundamento em Pedra e piloto sugerido (Impacto/tombamento, baixa complexidade + alto volume) — escolha certa.

## Pré-mortem: 3 jeitos de isso quebrar na produção (e o que fazer agora)

1. **Onboarding "prioridade desta semana" colide com a fundação.** O gatilho do agente 17 é `oportunidade.ganha` — que hoje só existe como estado no CRM (Bloco R1); o evento de saída (R9) é o passo 5 do contrato, e o envio de e-mail depende da F0.6 (pendente com a TI). **Caminho que funciona esta semana**: piloto com o CRM como registro do caso (ganho → checklist de implantação por família, dono e prazo — o CRM já tem isso) e e-mails em rascunho manual até a F0.6 chegar. Não esperar a fundação completa; não pular o registro do caso.
2. **Relógio sem SLA definido vira ruído.** "Parado há N dias" precisa de N por etapa (fábrica, agente de cargas, despachante, cliente). Se N não for definido, o relógio ou cala ou grita. **Ação**: na fundação (passo 0), definir SLA padrão por tipo de espera — o owner decide uma vez, os agentes usam.
3. **"Menos erros e horas de vida de volta" sem baseline não é provável.** Se não medir o estado atual antes, nenhuma afirmação de ganho será verificável depois. **Ação**: o passo 0 inclui 1–2 semanas de contagem simples (horas por processo, erros percebidos) — o relatório de gargalos mensal compara contra esse baseline.

## Riscos que encontrei além do pedido (protocolo de revisão)

1. **(MÉDIA, com evidência própria) Medição de LinkedIn vai esbarrar no mesmo 403 que eu esbarro.** O processo 15 assume coleta de métricas de redes sociais; na prática do meu monitor, `get_org_page_stats`/`get_share_stats`/`get_company_info` devolvem 403 (falta escopo `r_organization_social` no conector — reconexão feita 23/11 e o 403 persiste) e comentários não são legíveis via API. Impressões do perfil pessoal também não expostas. **Consequência**: o agente de medição precisa de fallback manual (owner copia do painel, como já fazemos) ou de conector/escopo novo — dimensionar isso antes de prometer medição automática de LinkedIn. Instagram/Facebook/YouTube/GA/RD a conferir caso a caso.
2. **(BAIXA) "Confirmação de recebimento" da proposta (processo 3) não é mensurável com M365 de forma confiável.** Graph não entrega recibo de leitura garantido. Reformular para cadência baseada em resposta: "toda proposta sem resposta em N dias entra na cadência; resposta = recebimento confirmado". Como está, o conferente checaria algo que o sistema não consegue observar.
3. **(BAIXA, consistência do documento) Contagem e numeração.** "São 16 processos" mas o Onboarding é o processo **17** — com ele, são 17 processos e 34 agentes (a tabela soma 16/32 sem ele). E a **ordem recomendada (seção 5) não inclui o Onboarding**, apesar de ser "prioridade desta semana" — inseri-lo logo após a fundação (piloto ShockWatch), antes ou junto do bloco de Vendas.
4. **(BAIXA) Cada par de agentes precisa nascer dentro do contrato v1.** Identidade (tipo=agente, humano_responsavel), autonomia padrão rascunho, campos de auditoria e eventos — o documento não cita o contrato; uma linha no padrão dos agentes fecha: "todo agente é registrado conforme o contrato comum CRM+ERP v1 e passa pelo portão do owner".
5. **(INFORMAÇÃO) Referências às Pedras não verificáveis ainda** — o arquivo `pedras-fundamentais.md` segue pendente de push; conferir a numeração ("Pedra 2") e o conteúdo citado quando chegar.
6. **(INFORMAÇÃO) Escopo fora do mapa assumido corretamente** — compras nacionais e prospecção ativa ficam de fora até existir mapa; a pergunta 8.1 ao owner segue aberta.

## Resposta às perguntas em aberto (seção 8) — o que cabe a mim

- **8.1 Compras nacionais / prospecção**: não tenho o mapa; se existir, o CC analisa no mesmo formato. Se não existir, desenhar com o owner depois do piloto de onboarding.
- **8.2 Limites de escala**: concordo com preenchimento gradual; enquanto vazios, tudo escala (comportamento seguro). Sugestão: começar só com **inadimplência** (dias e R$) — é a de menor discussão e destrava a cobrança (processo 11).
- **8.3 Agência de marketing**: não consta na minha memória além de "4 fornecedores sem medição" (análise da estrutura v1 do CC). Resposta é do owner.

## Próximo passo

Owner responde (ou já respondeu) as 3 perguntas da seção 8 → CC aplica os ajustes desta revisão no documento → push dos 4 arquivos → fundação (passo 0) vira briefing com SLAs e baseline. O piloto de Onboarding pode começar esta semana no formato do pré-mortem item 1 (CRM como registro, e-mails em rascunho).
