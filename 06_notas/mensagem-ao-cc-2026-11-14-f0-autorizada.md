# Mensagem ao CC — 14/11/2026 09:15 — F0 AUTORIZADA, pode começar

Bart aqui. **F0 AUTORIZADA pelo owner, com 4 ajustes** — briefing atualizado em `06_notas/briefing-f0-f1-ia-captura.md` (GitHub 577f301). Pode começar pela **F0.1–F0.5**:

1. **Modo inicial = `rascunho`** (aceito pelo owner) — default em `configuracoes.ia_modo`; nada sai sem clique humano.
2. **`horarios_executivo` virou `janelas_executivo`** — cadastro de janelas preferidas (dia, início–fim, ativo), editável pelo owner. Os horários oferecidos no e-mail são **janelas ∩ agenda real do Outlook (livre/ocupado)** via Graph — **nunca horário fixo sem conferir a agenda**. Se a agenda não responder, **nenhum horário é oferecido**: o e-mail vira rascunho com alerta. Nunca inventa horário.
3. **Não existe "esteira do relatório diário" nem integração M365 herdada** — o app no Entra ID entra na F0 como **entrega nova (F0.6)**: permissões **Mail.Read, Mail.Send (restrita à caixa do CEO), Calendars.ReadWrite, transcrições**, com **Application Access Policy restrita ao grupo "Comercial – IA"**, executada pelo **administrador de TI da AHM**. Credenciais por canal seguro, guardadas em secret do projeto. O CC não precisa fazer nada disso — é entrega organizacional; no código, só deixar o ponto de integração pronto (leitura de agenda + envio) para quando as credenciais chegarem.
4. **Conversa sem dor clara → fila do executivo do roteamento** (o vendedor responsável pelo modelo de compra), **NÃO transbordo** — transbordo continua sendo a fila técnica de especialista.

Critérios de aceite e pré-mortem atualizados no briefing (agora incluem: nenhum horário sem conferência de agenda real; sem dor clara → fila do executivo). Provas seguem o protocolo novo depois da implementação.

— Bart
