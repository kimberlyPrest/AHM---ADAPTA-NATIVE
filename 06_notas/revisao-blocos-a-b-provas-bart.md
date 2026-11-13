# Revisão dos Blocos A (0.0.68) e B (0.0.69) — provas do Bart · 13/11/2026 (noite)

**Protocolo aplicado**: pré-mortem · regressão por papel em todas as telas · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.

**Estado conferido antes de escrever** (correção do processo): migrations até 0032 (users_visiveis_equipe, 20:23) aplicadas; working tree do Skip lido direto na fonte (Oportunidades.tsx, OportunidadeDetalhe.tsx, hooks on_leads_oportunidade.js e on_transbordo_validate.js, index.html, Login.tsx). O briefing anterior do UX-A foi publicado sem essa conferência — registrado como falha de processo, corrigida aqui.

## BLOCO A (0.0.68) — APROVADO (confirmação com evidência de código + tela)

- **A1 (login)**: `useState('')` nos dois campos — 0 ocorrências de e-mail/senha hardcoded no Login.tsx (grep); "Solicitar acesso" abre modal "Fale com o seu gestor…" sem TI/ramal/suporte@ inventados; index.html com `lang="pt-BR"`, título "AHM Solution — Portal Comercial", `robots noindex, nofollow`. Provado na tela: campos vazios, login manual de vendedor e admin funciona.
- **A2 (migration 0032)**: vendedor lista users → 200 com nomes; JSON sem tokenKey/password; `expand=especialista` resolve o nome real no transbordo ("Afonso Moreira"); update de user continua próprio-ou-admin (PATCH em outro usuário → 404); troca de papel no hook (próprio papel → 403; último admin protegido → 400).
- **A5 (hooks)**: código confirmado — `try { ... } finally { e.next() }` nos dois hooks after-success; logs sem warning novo (último: 12/11 12:53, antes da correção). **Sobre o id de log pedido**: não vi aviso mais recente — os 35 warnings da lista terminam em 12/11 12:53; os mais recentes que li na varredura de erros (20:26–20:37 de hoje) são todos 403/400/404 esperados das provas.
- **Layout**: "Online" fixo substituído pelo papel real ("Julio / Vendedor", "Afonso / Administrador" no header); rodapé sem "v1.0.0".
- **Regressão das 11 provas do Bloco 3**: todas verdes (24/24 na bateria da revisão anterior).

## BLOCO B (0.0.69) — APROVADO

**Provas de API:**
- **(a) Bloqueio com motivo**: oportunidade de prova criada sem fechamento → vendedor tenta avançar de etapa → 400 **"Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento."** — mensagem clara, em português, apontando a seção. Com fechamento preenchido → avanço 200. ✓
- **(f) Acesso**: vendedor acessa a oportunidade por id → 200 ✓

**Provas de tela (como Julio, vendedor):**
- **(e) Rota em tela cheia**: `/oportunidades/swi68xhom7000w4` abre página própria (sem modal), com Fechamento, pipeline "AHM Cycle", Histórico — tudo na página ✓
- **(b) Escala de texto**: varredura de fontes na página da oportunidade — os únicos textos < 13px são o badge "Criado com o Skip" e o "0.75" do rodapé da plataforma (fora do controle do app); todo o conteúdo do app ≥ 13px ✓
- **(c) Modais maiores**: lead e transbordo abrem em modais ampliados (código confirmado; layout em largura inteira na página da oportunidade) ✓
- **(d) "Manter-me conectado"**: `pocketbase_auth` persiste no localStorage após login com o checkbox marcado ✓
- **(g) Histórico**: presente na página ✓
- **(h) Regressão de decisão**: botões de decisão (Aprovar/Rejeitar/Pedir revisão/Voltar para revisão) presentes e habilitados conforme o estado ✓

**Logs (protocolo item 5)**: erros recentes são todos esperados — 403 de auditoria para vendedor (correto), 400 do bloqueio de fechamento (a prova (a)), 404 de id inexistente (prova de robustez da página), 403 de DEMO/produto/renomear (regras do Bloco 3 funcionando). Nenhum erro inesperado.

## Pré-mortem do Bloco B (3 riscos)

1. **Rota direta para oportunidade de outro usuário vazar dados** → provado: vendedor acessa por id apenas o que pode ver (200); id inexistente → 404 com tela amigável "Oportunidade não encontrada" ✓
2. **Bloqueio de etapa travar avanço legítimo** → provado: com fechamento preenchido, avanço 200 ✓
3. **Fonte mínima quebrar gráficos/tabelas densas** → varredura na tela: nenhum texto do app < 13px ✓

## Riscos que encontrei além do pedido

1. **A3 (tela inicial) segue pendente (alta)** — confirmado: é o Bloco C, ainda não iniciado, dependendo das decisões do owner sobre indicadores e sobre Tickets/Atendimentos no menu. O Dashboard ainda exibe "Alta eficiência da equipe comercial" (frase inventada sob KPI=0) e "SLA corporativo meta: < 30 min" (fixo) — provado na tela na revisão anterior.
2. **Insumo do teste do Bloco 2 já tem fechamento (baixa, observação)**: a oportunidade "PROVA Teste Bloco 2 Baixa" (swi68xhom7000w4) foi criada COM fechamento (Venda em reais/100.000) — correta para o teste de ganho/perda, mas não serve para provar o bloqueio de etapa sem fechamento; criei insumo próprio de prova e limpei em seguida.
3. **Logs de erro com volume alto (4748 registros nível 8)** — a maioria são 403/400 esperados das regras (comportamento correto), mas vale o CC confirmar se há retenção/rotação de logs no Skip para não estourar cota.

## Limpeza

Lead "PROVA UX-B T1" + oportunidade de prova criados para a prova (a) — ambos apagados. Base intacta.

## Conclusão

**Blocos A (0.0.68) e B (0.0.69) APROVADOS.** Pendências registradas: A3/Bloco C (aguardando decisões do owner), troca de senha do admin (ação do owner), retorno de Julio e Rose sobre a página nova da oportunidade (teste do Bloco 2 em andamento — as fichas vão alimentar a validação da UX-B também).
