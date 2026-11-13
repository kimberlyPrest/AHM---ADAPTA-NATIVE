# Revisão completa da 0.0.68 (Bloco A) e 0.0.69 (Bloco B) — provas do Bart · 13/11/2026 (noite, consolidado)

**Protocolo aplicado**: pré-mortem · regressão por papel em todas as telas · varredura de segurança · dado inventado = defeito · logs · riscos fora do escopo · evidência antes de bloquear.
**Estado conferido antes**: migrations até 0032 aplicadas; working tree lido na fonte (Oportunidades.tsx, OportunidadeDetalhe.tsx, AuthContext.tsx, Login.tsx, index.html, hooks).

## RESULTADO: BLOCOS A E B APROVADOS — 32/32 provas de API + tela completa

## BLOCO A (0.0.68) — provas pedidas

- **(a) Nome do especialista visível**: vendedor lista colegas → 200 (Afonso, Beatriz, Carlos, Mariana, Rose…); transbordo atribuído a outra pessoa resolve "Afonso Moreira" via expand; JSON sem tokenKey/password ✓
- **(b) Vendedor edita outro usuário → 404** ✓
- **(c) Troca do próprio papel → 403** ✓
- **(d) Regressão das 11 provas do Bloco 3**: todas verdes (cadastro sem login 400; PROVA apagável só por admin com auditoria "administrador"; lead real 403; renomear real→PROVA 400; "Demolidora" aceita; auditoria/fechamentos 403; produto só admin; último admin protegido 400; DEMO só admin) ✓

**Segurança (varredura)**: 0 credenciais hardcoded; campos de login vazios; "Solicitar acesso" sem contatos inventados; index.html pt-BR + noindex; regras de auditoria intactas.

## BLOCO B (0.0.69) — provas pedidas

- **(a) Página abre pela fila**: clique na linha → `/oportunidades/swi68xhom7000w4` em tela cheia (sem modal); fila ganhou colunas **Etapa** e **Valor estimado**; contadores filtram ✓
- **(b) Botões travados sem fechamento e liberados depois**: API — sem fechamento → 400 "Para mudar a oportunidade de etapa, preencha o ambiente de fechamento e o valor estimado na seção Fechamento."; após salvar → 200 ✓. Na tela: a oportunidade de teste (com fechamento) mostra os botões habilitados e a seção Fechamento preenchida; o mecanismo de desabilitar com motivo ao lado + Fechamento destacado está no código (PipelineOportunidade) e a mensagem de bloqueio aparece na página ✓
- **(c) Decisões na página**: decisão no topo (Aprovar / Rejeitar / Pedir revisão / Voltar para revisão) conforme o estado; comentário obrigatório para rejeição e volta ✓
- **(d) Ganho, perda e reabertura**: ganho sem motivo → 400; com motivo (`resultado_motivo`) → 200; etapa fechada não muda → 400; reabrir sem justificativa (`reabertura_motivo`) → 400; com → 200; perda com motivo + concorrente → 200; **histórico de fechamentos com 3 registros** (ganha → reabertura → perdida) ✓
- **(e) Id inexistente → 404** com tela amigável "Oportunidade não encontrada" ✓
- **(f) "Manter-me conectado" marcado E desmarcado**: código revisado (AuthContext) — desmarcado grava `ahm_nao_lembrar=1` + cookie de sessão `ahm_sessao_aberta` (sem validade, some ao fechar o navegador); ao reabrir, flag presente + cookie ausente = `authStore.clear()`. Provado na tela: login desmarcado → flag gravada + cookie presente na aba; login marcado → flag removida ✓ (mecanismo correto: o token fica no localStorage mas a sessão morre ao fechar o navegador)
- **(g) Varredura visual**: em 1280px (notebook) — sem overflow horizontal; nenhum texto do app < 13px (só o "0.75" do badge do Skip, fora do app); classes responsivas (lg:) presentes para telas grandes; limite de 1.280px removido do código (max-w-[420px] apenas na lateral) ✓
- **(h) Regressão dos Blocos 2, 3 e A**: proteção de exclusão (403 em real, PROVA apaga com auditoria), checklist/fechamento/painel do Bloco 2, regras de papel do Bloco 3, nomes de colegas do Bloco A — todas verdes na bateria de 32 ✓

**Layout confirmado na tela**: decisão no topo, pipeline horizontal "AHM Cycle", Fechamento e Resultado sempre visíveis na lateral, histórico em linha do tempo, modais de lead/transbordo maiores.

## Logs (protocolo item 5)

- Último warning de `e.next()`: **12/11 12:53** — nenhum mais recente (confirma o que o CC relatou; os 35 warnings da lista terminam nessa data)
- Erros recentes (20:26–20:37): todos esperados — 403 de auditoria para vendedor, 400 do bloqueio de fechamento, 404 de id inexistente, 403 de DEMO/produto/renomear (regras funcionando)
- **Observação (baixa)**: 4.748 registros de erro nível 8 acumulados — a maioria esperada (regras), mas vale confirmar rotação/retenção no Skip

## Pré-mortem (3 riscos por bloco)

**Bloco A**: (1) view de nomes vazar dados sensíveis → provado que não (JSON público apenas); (2) login quebrar sem pré-preenchimento → provado na tela; (3) e.next() duplo → código revisado + logs limpos.
**Bloco B**: (1) rota direta vazar dados de outro usuário → vendedor acessa só o que pode; 404 amigável; (2) bloqueio travar avanço legítimo → com fechamento, 200; (3) fonte mínima quebrar tabelas densas → varredura sem texto < 13px no app.

## Riscos que encontrei além do pedido

1. **Bloco C (alta, pendente do owner)**: dono da oportunidade, motivos de perda fixos, meta de negócios, etapa de negociação, moedas separadas, Tickets/Atendimentos no menu — e o risco apontado pelo CC é real: **só há 2 oportunidades reais**, os gráficos do "Desempenho" vão nascer vazios; a importação do histórico do Pipedrive merece bloco próprio (com deduplicação contra a base — lição do Bloco 5).
2. **"Manter-me conectado" desmarcado mantém o token no localStorage (baixa)**: o mecanismo é correto (cookie de sessão + flag), mas o token continua no disco até a próxima abertura. Risco baixo em desktop corporativo; se quiser rigor maior, o CC pode migrar para sessionStorage quando desmarcado.
3. **Insumo do teste do Bloco 2 já tem fechamento (baixa, observação)**: correto para o roteiro de ganho/perda, mas não prova o bloqueio visual de etapa sem fechamento — se Julio/Rose testarem isso, a tela mostrará os botões habilitados (comportamento correto para o estado deles).
4. **Contadores da fila de oportunidades somam PROVA (baixa)**: as oportunidades de teste do Bloco 2 aparecem nos contadores ("Total 7") até a limpeza — cosmético, some com a limpeza pós-teste.

## Limpeza

2 leads + 2 oportunidades de prova criados e apagados; "Demolidora Comércio AB" arquivada com motivo. Base intacta (Aroldo íntegro).

## Conclusão

**Blocos A (0.0.68) e B (0.0.69) APROVADOS.** Aguardando: retorno de Julio e Rose (Bloco 2 + página nova), decisões do owner para o Bloco C, troca de senha do admin (em andamento pelo owner).
