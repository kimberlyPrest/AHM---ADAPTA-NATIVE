# AP-2026-11-10-2019 — SDK PocketBase: campos undefined não limpam valores em update

- Status: capturado
- Escopo: projeto do cliente
- Task/SPEC: F1-T08 / SPEC 1.3 (reprocessamento de qualificação, modal Editar Lead)
- Sinal: no SDK JS do PocketBase, update() com campo `undefined` OMITE o campo do payload — o valor antigo permanece na base. Um formulário de edição que usa `campo: form.campo || undefined` para "limpar" não limpa nada: o usuário apaga o texto, salva, e o valor anterior continua (bug silencioso, só aparece no teste do fluxo completo).
- Evidência: teste humano do owner — apagar a dor no modal Editar e requalificar voltava "qualificado" porque a dor antiga permanecia (dor: undefined não era enviada); corrigido em Skip 0.0.25 enviando string vazia explicitamente (`dor: editForm.dor`), provado via API (PATCH dor vazio → campo limpo '').
- Regra reutilizável: em formulários de edição PocketBase, campos opcionais apagados devem ser enviados como '' (string vazia) ou null explícito — nunca undefined. Filtros de payload devem remover apenas undefined de campos que NÃO devem ser tocados.
- Quando aplicar: todo modal de edição do projeto (leads, produtos, oportunidades, transbordo).
- Quando não aplicar: criação de registros (undefined = campo não preenchido é correto).
- Confiança: alta — bug reproduzido pelo owner no teste humano e correção provada via API.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
