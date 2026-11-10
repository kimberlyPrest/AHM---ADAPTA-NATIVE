# Estado atual — Adapta Cliente

- task_id: F1-T05
- champion: Rose
- spec: 04_fase-atual/specs/spec-1.2-leads-manuais-importacao.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada 2026-11-10 15:31 BRT — "pode implementar" (owner Afonso), com amostra real: base RD Station (15.387 contatos, UTF-16/TAB)
- teste_humano: pendente
- verificacao_automatica: passou — Skip 0.0.14 (hash fa91f81), QA completo OK; provas: CSV misto (4 linhas: 3 completos→novo, 1 sem empresa→rascunho pendente, produto resolvido por nome→Shockwatch, urgência/origem aplicadas); arquivo ilegível→toast "Cabeçalhos não reconhecidos" e nada importado; recorte real RD UTF-16/TAB→prévia 6 válidas+1 duplicada na base (Aroldo/Stellantis detectada corretamente)→6 criados com cargo/telefone em observações; base limpa após provas
- aprendizado: pendente
- ultima_acao: F1-T05 implementada (src/lib/csvImport.ts + leadsService.importarLote + UI Importar CSV em /leads); provas via navegador com CSV misto, arquivo ruim e recorte real do RD
- proxima_acao: aguardar teste humano (Afonso importa CSV na tela e confirma prévia/relatório)
- atualizado_em: 2026-11-10T15:55:00-03:00
