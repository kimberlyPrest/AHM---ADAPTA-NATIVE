# AP-2026-11-12-2245 — Limpar campo controlado do React na prova de tela

**Contexto**: revisão do Bloco 4 — prova de limpar o telefone no Editar Lead.

**O que aconteceu**: `fill` do navegador com string vazia não disparou o onChange do React — o modal fechou como "salvo" mas o campo continuou com o valor antigo na base. O caminho que funcionou: setter nativo (`Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set`) + evento `input` com bubbles, depois clicar em Salvar.

**Sinal reutilizável**: em prova de tela com React, esvaziar campo controlado exige o setter nativo + evento `input`; `fill` vazio só muda o DOM, não o estado. Confirmar o efeito na API (não só no modal) antes de reportar.
