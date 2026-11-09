# AP-2026-11-09-1350 — Padrão de nova tela React no Skip (página + rota + navegação)

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T02 / SPEC 1.1 (CA-1-001, CA-1-003)
- Sinal: nova tela segue 4 pontos de integração obrigatórios — arquivo em src/pages/, rota em src/App.tsx (bloco de rotas protegidas), item em navItems de src/components/Layout.tsx (com ícone lucide importado) e título em getPageTitle do Layout
- Evidência: Skip 0.0.5 (hash 7f46b2a) com QA completo OK na primeira tentativa; tela /produtos acessível (HTTP 200) e ciclo ativar/inativar verificado na tela e via API
- Regra reutilizável: ao criar tela no Skip React, alterar os 4 pontos na mesma mudança e reusar o serviço em src/services/ já existente; seguir tokens visuais (#DE6464, #030507, #FAF7F2, cards rounded-2xl border-[#E5E7EB]) e padrão de toast do app
- Quando aplicar: toda task de tela nova (F1-T03 em diante: formulário de lead, painel KPIs, timeline)
- Quando não aplicar: mudança só de backend (migrations/hooks) ou ajuste em tela existente
- Confiança: alta — build e QA passaram de primeira, tela verificada em execução
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
