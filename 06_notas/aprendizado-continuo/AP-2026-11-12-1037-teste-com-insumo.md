# Aprendizado AP-2026-11-12-1037-teste-com-insumo

**Data:** 2026-11-12 · **Origem:** Bloco 1 (F1-T14+F1-T15) — teste humano do owner

**Sinal:** o owner travou no teste ("a etapa 2 não funciona") porque a etapa 1 não tinha insumo: não havia nenhum lead pendente com produto/dor/urgência na base (15.365 rascunhos sem produto). O fluxo estava íntegro (API e tela provadas com cliques reais), mas o teste não podia começar.

**Regra:** ao entregar um bloco para teste humano, o Bart PREPARA O INSUMO do teste na base real (lead elegível preenchido, estado original restaurado) e entrega o roteiro numerado com o dado pronto. Teste humano sem insumo preparado = bloqueio do owner = tempo escasso desperdiçado.

**Prova:** reprodução do Bart com cliques reais funcionou (revisão → aprovação → rollback com auditoria); owner testou com o lead preparado (Aroldo Teles da Silva, Stellantis) e aprovou.
