# Mensagem ao CC — 14/11/2026 17:00 — CONSOLIDADO: tudo que você precisa saber agora (F0 entregue e aprovado, roteamento por família, perguntas em aberto)

Bart aqui. O owner pediu para consolidar **todas as dúvidas e informações em uma única mensagem** — esta substitui as anteriores como referência única. Leia inteira antes de qualquer ação.

---

## 1. Estado atual — onde estamos

- **F0.1–F0.5 (0.0.74): ENTREGUE POR VOCÊ E APROVADA POR MIM** — 31/31 provas de API verdes (re-executadas em 14/11, exceto o critério de roteamento, ver item 3). Relatório: `06_notas/revisao-f0-ia-provas-bart.md`. As 2 notas baixas do relatório estão RESOLVIDAS: re-teste com `dia_semana=9` → 400 correto; POST forjado em `ia_envios` → 403 (o PATCH/DELETE com id inexistente → 404 é o mesmo padrão do D4 do Bloco 3, aceito).
- **Bloco D (Pipedrive): SUSPENSO pelo owner** — histórico não confiável; NADA importado; rota/tela ficam prontas. Não trabalhe nele.
- **Fase 2**: Blocos 1, 2, 3, A, B, C, D e UX-A/B/C aprovados. Teste com usuários (Julio/Rose) em andamento.

## 2. Decisão do owner: roteamento por FAMÍLIA (migration 0036)

**Rotear por FAMÍLIA DE PRODUTO, não mais por modelo de negócio.** Motivo: a dor que o cliente descreve na 1ª conversa aponta para uma família; o modelo de negócio (assinatura/cíclico/pontual) só é decidido na proposta.

**Desenho aprovado em princípio:**
1. Regra por `familia` (texto, alinhada às 8 famílias da emenda 1.1a) → `vendedor`.
2. **Área de interesse como reserva** — se a família não ficar clara na conversa, roteia pela área de interesse do lead (Qualidade no Transporte | Cadeia do Frio | Segurança e Logística, emenda 1.2a). **Regra específica vence genérica**: família vence área de interesse.
3. **Sem identificação segura → fila do administrador** (não primeiro vendedor, não transbordo técnico).
4. **Seed sai da planilha do owner** — `05_dados/ia-vendas/familias-roteamento.xlsx` (família, executivo, expert do Adapta, dor nas palavras do cliente, perguntas, materiais, área de reserva). **Essa pasta NUNCA vai para o GitHub** (.gitignore, dados da empresa). Quando o owner entregar, EU te passo o conteúdo consolidado.
5. Continua valendo da F0 (já provado): CRUD de cadastro só admin; **regra ativa sem vendedor → 400**; edição sem deploy; `aguardando_cadastro` para executivo sem conta (Paulo).

**Detalhes que eu vou provar na entrega:**
- `regras_roteamento`: trocar `modelo_negocio` (rel) por `familia` (texto max 120) + `area_interesse` (select dos 3 valores, opcional) — ou coleção nova; sua escolha, desde que "específica vence genérica" fique clara no código e na tela.
- Tela `/ia`: tabela de roteamento lista famílias (reserva por área visível) e a fila do admin como destino explícito.
- Sem dado inventado: família sem executivo na planilha → regra inativa com `aguardando_cadastro` (mesmo padrão do Paulo).
- **As regras antigas por modelo de negócio: NÃO apagar sem pedir** — proponha na entrega se remove ou mantém inativas até a 0036 ser validada.

**ORDEM: implemente a 0036 SÓ DEPOIS da autorização explícita do owner** (ele está preenchendo a planilha agora). Até lá, não comece.

## 3. O que NÃO está em revisão (já provado — não mexer sem necessidade)

Tudo da F0 exceto a tabela de roteamento: permissões das 3 coleções, `ia_envios` impenetrável (só servidor grava), validações (fonte obrigatória, dia_semana 0–6, início<fim, ia_modo), prévia de horários (`agenda_indisponivel` → nenhum horário), modo rascunho.

## 4. Ajustes do owner na F0 (já incorporados no briefing — relembrando)

1. Modo inicial **rascunho** (aceito).
2. `janelas_executivo` = janelas preferidas; horários oferecidos = janelas ∩ agenda real do Outlook — **nunca horário fixo sem conferir a agenda**; agenda indisponível → nenhum horário + alerta.
3. **Não existe "esteira do relatório diário" nem integração M365 herdada** — o app no Entra ID (Mail.Read, Mail.Send só na caixa do CEO, Calendars.ReadWrite, transcrições; Application Access Policy → grupo "Comercial – IA") é a **F0.6, entrega nova executada pelo administrador de TI da AHM** — não é seu trabalho; só deixe os pontos de integração prontos (leitura de agenda + envio).
4. **Conversa sem dor clara → fila do executivo do roteamento** (vendedor do modelo de compra), **NÃO transbordo** (transbordo = fila técnica de especialista).

## 5. Pendências do owner (não bloqueiam você, exceto onde dito)

| Pendência | Destrava | Status |
|---|---|---|
| Planilha de famílias preenchida | Seed da 0036 | Owner preenchendo |
| Autorização da 0036 | Implementação do roteamento por família | Aguardando planilha |
| APIs RD Station + Envya | F1.1/F1.2 | E-mail de solicitação pronto; owner envia ao especialista |
| Especialistas, vídeos, business cases | F1.3 (base de conhecimento) | Owner (planilha tem abas para isso) |
| Cadastro do Paulo | Regra pontual + F1 | Owner passa nome+e-mail; Bart cria via API |
| App Entra ID (F0.6) | Envio M365 + agenda real | Administrador de TI da AHM; credenciais por canal seguro |
| Bônus/margens/pesos de impacto | Futuro | Chegam depois; tabela editável pelo admin |

## 6. Perguntas ao suporte do Skip (em aberto — a resposta decide a arquitetura da F1)

O owner quer saber, sobre o `$ai.chat`:
- **(a)** Qual modelo de IA está por trás dos níveis "fast" e "reasoning" — fabricante e versão?
- **(b)** É possível escolher um modelo específico (ex.: Claude da Anthropic) ou usar chave própria (BYOK)? Se sim, como?
- **(c)** Quais limites de tamanho por chamada (tokens entrada/saída) — pensando em transcrições longas de reunião e e-mails?
- **(d)** Onde os dados enviados à IA são processados (região/provedor) e por quanto tempo ficam guardados — para a política de LGPD?

A resposta decide se a IA de vendas segue **100% dentro do Skip** ou precisa de **serviço à parte** (a documentação do Skip proíbe chamar fornecedores de IA diretamente pelos hooks). Se você tem canal direto com o suporte, adianta essas perguntas; senão, o owner pergunta.

## 7. Próximos passos (resumo)

1. **Você**: aguarda a autorização da 0036 → implementa roteamento por família + seed → me avisa para as provas.
2. **Owner**: preenche a planilha → autoriza a 0036 → resolve pendências da tabela do item 5.
3. **Eu**: provo a 0036 (família vence área, reserva, fila do admin, permissões, regressão por papel) e publico o relatório.

Dúvidas? Responde neste arquivo ou me aciona pelo owner.

— Bart
