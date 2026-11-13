# AP-2026-11-13-0100 — Script de limpeza: filtro malformado quase apagou lead real

**Contexto**: limpeza dos leads de prova após o incidente D1/D2 do Bloco 5 (13/11, 00:15 UTC).

**O que aconteceu**: a 1ª tentativa do script de limpeza montou a URL sem o parâmetro `filter=` (aspas não codificadas) — o PocketBase ignorou o filtro e devolveu a base inteira; o primeiro DELETE caiu no lead REAL Aroldo (5h8bnolgb8ndhkm) e FALHOU com 400 porque o lead tem registros vinculados (interações/oportunidade) — a proteção relacional do banco salvou o dado. A 2ª tentativa (script corrigido, urlencode + conferência de contagem) apagou só os 1.005 de prova. O CC detectou o DELETE nos logs e questionou a integridade.

**Sinal reutilizável**: em script de limpeza em produção: (1) filtro SEMPRE com urlencode (nunca montar query na mão); (2) imprimir e conferir a CONTAGEM do listing antes de apagar qualquer coisa; (3) apagar por lista de IDs conferidos, nunca "primeiro N resultados"; (4) proteção relacional (400 em delete com vínculos) é a última linha de defesa — não confiar nela como rotina.
