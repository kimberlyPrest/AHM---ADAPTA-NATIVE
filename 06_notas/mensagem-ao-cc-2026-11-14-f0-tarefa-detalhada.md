# Mensagem ao CC — 14/11/2026 09:20 — Explicação da próxima tarefa (F0.1–F0.5)

Bart aqui. O owner autorizou a **F0 da IA de vendas** e pediu para eu explicar a próxima tarefa em detalhe. Fonte canônica: `06_notas/briefing-f0-f1-ia-captura.md` (GitHub 577f301). A tarefa é **F0.1–F0.5** — fundação da IA, sem NENHUMA dependência externa (nada de API do RD/Envya, nada de credencial M365 — isso é F1/F0.6, depois).

## Contexto (o porquê)

O owner aprovou: quando o RD Station pontua um lead, a IA lê a conversa do Envya e envia, em nome do CEO, um e-mail com prova de entendimento da dor, 1–3 perguntas de qualificação, 2–3 horários do executivo, vídeo/business case quando fizer sentido e o especialista técnico do produto. Roteamento: Julio (recorrente+assinatura), Paulo (pontual).

A F0 constrói **os cadastros e a auditoria** que a geração do e-mail (F1.3) vai consumir. O princípio da emenda 1.3a vale para tudo: **regra em cadastro editável pelo owner, nunca em código**; **toda recomendação cita a fonte**; **IA nunca inventa dado** — se o cadastro não tem, o e-mail não leva.

## O que construir

### F0.1 — Coleção `base_conhecimento`
Campos sugeridos: `tipo` (produto | familia | faq | caso | video | business_case | especialista), `titulo`, `conteudo`, `produto` (rel), `familia` (rel), `fonte` (URL ou referência de documento — **obrigatória**), `ativo` (bool).
- CRUD **só admin**; a IA lê server-side (hook/route), nunca pelo frontend público.
- Tela simples de gestão no admin (lista + editar).
- Seed inicial: nada além do que já existe — produtos e famílias de diagnóstico já estão no catálogo; especialistas entram quando o owner passar os nomes (pode deixar a coleção vazia por enquanto).

### F0.2 — Coleção `regras_roteamento`
Campos: `modelo_compra` (recorrente | assinatura | pontual), `vendedor` (rel users), `ativo`, `prioridade`.
- Seed: recorrente→Julio, assinatura→Julio, pontual→Paulo (**Paulo ainda não tem conta — criar a regra apontando para o usuário quando ele existir; se o seed não resolver sem o usuário, deixar a regra pontual inativa com flag "aguardando cadastro"**).
- Tela de edição para o owner (pode entrar em Configurações). Mudar regra **sem deploy**.

### F0.3 — Coleção `janelas_executivo`
Campos: `dia_semana` (0–6), `hora_inicio`, `hora_fim`, `ativo`.
- **Atenção ao ajuste do owner**: são **janelas preferidas**, não horários fixos. O cruzamento com a agenda real do Outlook (livre/ocupado) acontece na F1.3 — aqui só o cadastro. Mas **deixe o ponto de integração preparado**: uma função server-side `horarios_disponiveis(janelas, agenda)` que recebe as janelas do cadastro e a lista de ocupados e devolve 2–3 horários livres — com teste unitário cobrindo: janela vazia, agenda indisponível (retorna vazio + flag de erro), sobreposição parcial.
- Regra já agora: **se a agenda não responder, nenhum horário é oferecido** (o consumidor dessa função na F1.3 vai gerar rascunho com alerta, nunca horário inventado).

### F0.4 — Coleção `ia_envios` (auditoria)
Campos: `lead` (rel), `gatilho`, `rascunho_email`, `versao_enviada`, `status` (rascunho | aprovado | enviado | erro | descartado), `autorizado_por` (rel users, só quando manual), `conversa_resumo`, `criado_em`.
- **Escrita só pelo servidor** (API rules: sem create/update/delete por cliente; leitura só admin). Mesmo padrão das coleções de auditoria do Bloco 2 (`oportunidade_fechamentos`, `exclusoes_auditoria`).

### F0.5 — Modo de operação
- `configuracoes.ia_modo`: `desligada` | `rascunho` | `automatica` — **default e valor inicial: `rascunho`** (aceito pelo owner).
- No modo `rascunho`, a IA gera o e-mail, grava em `ia_envios` com status `rascunho` e notifica o owner no app — **nada sai sem clique humano**. (A geração em si é F1.3; aqui o que importa é o modo existir, ser editável só por admin e o default correto.)
- A fila de rascunhos aparece para o owner (pode ser um card na Minha mesa quando houver rascunhos pendentes).

## Ajustes do owner que você precisa respeitar (já no briefing)

1. Modo inicial **rascunho** (não automático).
2. `janelas_executivo` = janelas preferidas ∩ agenda real do Outlook — **nunca horário fixo sem conferir a agenda**.
3. **Não existe "esteira do relatório diário" nem integração M365 herdada** — o app no Entra ID (Mail.Read, Mail.Send só na caixa do CEO, Calendars.ReadWrite, transcrições; Application Access Policy → grupo "Comercial – IA") é **entrega nova da F0 (F0.6), executada pelo administrador de TI da AHM** — não é trabalho seu; só deixe os pontos de integração prontos.
4. **Conversa sem dor clara → fila do executivo do roteamento** (o vendedor do modelo de compra), **NÃO transbordo** (transbordo = fila técnica de especialista).

## Fora do escopo desta tarefa

Webhook do RD, leitura do Envya, geração do e-mail, envio — tudo F1, destravado pelas pendências do owner (APIs, especialistas, vídeos, Paulo, execução do F0.6 pelo TI).

## Critérios de aceite (minhas provas depois)

1. CRUD das 3 coleções de cadastro só admin; vendedor → 403 com mensagem corporativa.
2. `ia_envios` não aceita escrita do cliente (POST forjado → 403); leitura só admin.
3. `ia_modo` default `rascunho`; só admin muda.
4. Regras de roteamento e janelas editáveis pelo owner **sem deploy**.
5. `horarios_disponiveis(janelas, agenda)` testada: agenda indisponível → vazio + flag de erro (nunca horário).
6. Regressão por papel em todas as telas + logs limpos.

Seguimos o ritmo de sempre: implementa → QA → me avisa que eu rodo as provas com o protocolo novo → relatório → teste humano/aceite do owner.

— Bart
