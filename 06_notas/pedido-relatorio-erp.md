
Pedido de relatório ao Claude Code do ERP (24/11/2026)
Como usar: copiar o bloco abaixo e colar no Claude Code do VS Code, dentro do projeto do ERP. Trazer a resposta (arquivo ou texto) para a sessão do CRM.

Preciso de um relatório de situação deste projeto (ERP) para avaliar se ele está pronto para ser operado por humanos E por agentes de IA, coordenados por um agente "Chief of Staff" que também vai atuar no CRM da AHM. NÃO altere nada no código: só leitura. Grave o relatório em um arquivo `relatorio-erp-para-crm.md` na raiz do projeto e responda em português.

Baseie cada resposta no CÓDIGO (cite arquivo e linha). Onde não souber, escreva "não verificado" — não suponha.

1. Visão geral
   - Objetivo do ERP, módulos existentes e módulos planejados.
   - Stack: linguagem, framework, banco, onde roda (local, nuvem, plataforma), como é publicado.
   - Estado: o que está pronto, em andamento e só planejado. Data do último trabalho.

2. Modelo de dados
   - Entidades/tabelas principais e campos-chave.
   - Entidades que se sobrepõem ao CRM: cliente/empresa, contato, produto, pedido, proposta, oportunidade, usuário. Qual é o identificador de cada uma?

3. Avaliação contra os 10 requisitos (para cada um: ✅ atende / 🟡 parcial / 🔴 não atende + evidência no código + o que faltaria)
   R1 Identidade própria: existe usuário do tipo "agente" separado de humano, com credencial própria e humano responsável?
   R2 Regras no servidor: toda regra de negócio, estado derivado e histórico roda no back-end (fazer pela API = fazer pela tela)?
   R3 Permissão mínima: permissões por papel/usuário impostas no servidor?
   R4 Autonomia graduada: existe modo desligado / rascunho com aprovação humana / automático por tipo de ação?
   R5 Auditoria uniforme: cada alteração grava quem fez, se humano ou agente, origem, motivo e fonte?
   R6 Data do fato separada da data do registro?
   R7 Idempotência: repetir a mesma chamada duplica registro?
   R8 Erros: as respostas trazem código estável (para máquina) + mensagem legível em pt-BR?
   R9 Eventos: o sistema publica mudanças (webhook, fila, feed) para outros sistemas/agentes reagirem?
   R10 Supervisão humana: há tela para ver o que cada agente fez, aprovar pendências e pausar um agente?

4. API
   - Lista de endpoints/rotas (método, caminho, o que faz, quem pode chamar).
   - Como é a autenticação para chamadas de máquina (token, chave, OAuth)?
   - Existe documentação da API (OpenAPI/Swagger)?

5. Integrações
   - Integrações existentes ou planejadas (fiscal, bancos, Microsoft 365, CRM, outros).
   - Como o ERP trocaria dados com o CRM (que roda no Skip Cloud / PocketBase)?

6. Riscos e pendências
   - Dívidas técnicas, partes frágeis, decisões em aberto, dados sensíveis (LGPD).

7. Resumo executivo (no início do arquivo, até 10 linhas): o quanto falta para o ERP ser operável por agentes, e as 3 primeiras ações recomendadas.
