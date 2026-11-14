# Mensagem ao CC — 14/11/2026 16:55 — Roteamento por FAMÍLIA (migration 0036): desenho aprovado em princípio, implementar APÓS autorização do owner

Bart aqui. O owner revisou o critério de roteamento da IA de vendas e a migration 0036 já tem desenho aprovado **em princípio** — **implemente só depois da autorização explícita dele** (ele está preenchendo a planilha de famílias agora; ela é o insumo do seed).

## A decisão

**Rotear por FAMÍLIA DE PRODUTO, não mais por modelo de negócio.** O motivo: a dor que o cliente descreve na primeira conversa aponta para uma família; o modelo de negócio (assinatura, cíclico, pontual) só é decidido na proposta — não roteia mais.

## O desenho aprovado em princípio

1. **Regra por família** — `familia` (texto, alinhado às 8 famílias da emenda 1.1a) → `vendedor`. A tabela atual por `modelo_negocio` sai (ou vira reserva — ver item 2).
2. **Área de interesse como reserva** — se a família não ficar clara na conversa, o lead é roteado pela **área de interesse do lead** (Qualidade no Transporte | Cadeia do Frio | Segurança e Logística, campos da emenda 1.2a). **Regra específica vence genérica**: família vence área de interesse.
3. **Sem identificação segura → fila do administrador** — lead sem família identificada E sem área de interesse válida vai para a fila do admin (não para o primeiro vendedor, não para transbordo técnico).
4. **Seed pela planilha do owner** — ele está preenchendo `05_dados/ia-vendas/familias-roteamento.xlsx` (família, executivo, expert do Adapta, dor nas palavras do cliente, perguntas, materiais, área de interesse de reserva). **Essa pasta NUNCA vai para o GitHub** (está no .gitignore, dados da empresa). Quando ele entregar, o seed da 0036 lê dela — me avise que eu te passo o conteúdo consolidado (ou o owner cola direto na sua sessão).
5. **O que continua valendo da F0** (já provado, 31/31): permissões (CRUD de cadastro só admin), **regra ativa sem vendedor → 400**, edição sem deploy, `aguardando_cadastro` para executivo sem conta (Paulo).

## Detalhes de implementação que eu vou provar

- `regras_roteamento`: trocar `modelo_negocio` (rel) por `familia` (texto, max 120) + `area_interesse` (select dos 3 valores, opcional) como reserva — ou coleção nova; sua escolha, desde que a regra específica vence a genérica fique clara no código e na tela.
- Validação mantida: ativa sem vendedor → 400.
- Tela `/ia`: tabela de roteamento passa a listar famílias (com a reserva por área visível) e a fila do admin como destino explícito.
- Sem dado inventado: se a planilha não trouxer executivo para uma família, a regra nasce inativa com `aguardando_cadastro` (mesmo padrão do Paulo).
- As regras antigas por modelo de negócio: **não apagar dados do owner sem pedir** — propor na entrega se a tabela antiga é removida ou mantida inativa até a 0036 ser validada.

## Perguntas ao suporte Skip (em aberto — não bloqueiam a 0036)

O owner quer saber, sobre o `$ai.chat`: (a) qual modelo está por trás dos níveis fast/reasoning (fabricante e versão); (b) se dá para escolher modelo específico (ex.: Claude) ou usar chave própria; (c) limites de tamanho por chamada (transcrições longas, e-mails); (d) onde os dados são processados e por quanto tempo ficam guardados (LGPD). Se você tiver canal direto com o suporte, adianta; senão eu registro no relatório e o owner pergunta.

## Ordem

1. Owner autoriza a 0036 (depois de preencher a planilha).
2. Você implementa a troca de roteamento + seed.
3. Eu provo: roteamento por família (específica vence genérica), reserva por área, fila do admin, permissões, regra ativa sem vendedor → 400, edição sem deploy, regressão por papel.

— Bart
