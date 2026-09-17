# Fase 1 — Tarefas

<!-- fase-format:2 -->

Cada linha é uma tarefa da Jornada de Execução. **Tudo que cabe num card cabe nesta linha** — se um
campo não estiver aqui, ele não tem como ser preenchido, porque é este arquivo que cria a tarefa.

```
- [ ] Título da tarefa @responsável !30/09/2026 #projeto [interno]   <!-- id:… -->
      > descrição da tarefa, uma ou mais linhas
  - [ ] subtarefa (basta indentar 2 espaços)                         <!-- id:… -->
    - [ ] sub-subtarefa (indente mais 2)                             <!-- id:… -->
```

| marcador | o que define | se você não escrever |
|---|---|---|
| `- [ ]` / `- [/]` / `- [x]` | a fazer / em andamento / concluída | a fazer |
| `@nome` | responsável (`@"Nome Composto"` com aspas) | fica **sem responsável** |
| `!dd/mm/aaaa` | prazo | fica **sem prazo** |
| `#projeto` / `#aculturamento` | tipo | Projeto de IA |
| `[interno]` | o cliente **não** vê esta tarefa | o cliente vê |
| `> texto` na linha de baixo | descrição (aparece ao abrir o card) | sem descrição |
| indentar 2 espaços | vira subtarefa da tarefa acima (vale em qualquer profundidade) | tarefa de topo |

Os marcadores só valem **no fim da linha** — `Revisar #3 do contrato` continua sendo um título.
Um título que TERMINA na forma de um marcador sai escapado com `\\` (`Ligar para \\@joao`); a barra é
só para o parser e nunca aparece no card. Você não precisa escrever isso à mão.
Marque `[x]` para concluir e adicione linhas novas à vontade: elas entram no quadro na próxima
sincronização e voltam aqui com o `<!-- id:… -->` preenchido. **Não apague o marcador de id** das
tarefas que já têm um.

- [ ] 3. Mapear lógica de qualificação e recomendação de produto @"Kimberly Prestes" [interno]  <!-- id:503b3c48-8e92-43a0-abfe-1196e38ae9bc -->
- [ ] Criar estrutura de catálogo de produtos sem integração @"Kimberly Prestes" [interno]  <!-- id:2748475a-9901-41f4-863b-f601127c18ab -->
  > Permitir cadastro manual de produto/família com ICP, dor, escopo, perguntas, timing, baixa/alta complexidade e especialista.
- [ ] Criar entrada manual/importável de leads @"Kimberly Prestes" [interno]  <!-- id:94260a80-f0fb-453a-a037-e438e37e5084 -->
  > Permitir operar a Fase 1 por formulário interno, planilha ou CSV, sem API.
- [ ] Implementar classificação inicial do lead @"Kimberly Prestes" [interno]  <!-- id:5878bd13-e41e-44d8-901c-eb433419e653 -->
  > Classificar lead como qualificado, pendente, fora do recorte ou transbordo, sempre com justificativa.
- [ ] Criar fila de oportunidades com aprovação humana @"Kimberly Prestes" [interno]  <!-- id:8c5ae272-3ca1-4cd7-8319-363e6fd9d18e -->
  > Exibir oportunidade sugerida e permitir aprovar, rejeitar ou pedir revisão.
- [ ] Criar painel mínimo de K1/K2/K3 @"Kimberly Prestes" [interno]  <!-- id:fae1d54c-57c7-4142-b2b7-f68c54f8b41a -->
  > Mostrar baseline e métricas da operação manual/local da Fase 1.
- [ ] F1-T01 — Criar modelo e validações do catálogo de produtos @Ethos [interno]  <!-- id:574edb71-96a6-400d-8bef-d88737c19f27 -->
- [ ] F1-T02 — Criar tela/lista de catálogo com ativar/inativar @Ethos [interno]  <!-- id:a905ef54-aa20-4a8b-9cac-8b688564872d -->
- [ ] F1-T03 — Criar fixture de produto exemplo para demonstração @Ethos [interno]  <!-- id:0e18e12b-463f-4a1e-913d-a1d339b4d682 -->
- [ ] F1-T04 — Criar modelo e formulário de lead manual @Ethos [interno]  <!-- id:086a8513-6e2d-49ca-be42-12e1d25cb081 -->
- [ ] F1-T05 — Criar importação CSV com prévia e relatório de erros @Ethos [interno]  <!-- id:37b1f46d-690f-4e87-9021-268c4423fbdf -->
- [ ] F1-T06 — Criar sinalização de duplicidade local @Ethos [interno]  <!-- id:b1b7f98e-7392-4109-8ad6-918567f87e36 -->
- [ ] F1-T07 — Implementar estados e regras iniciais de qualificação @Ethos [interno]  <!-- id:7d5e2865-5458-47ca-bd4a-c971bc02ad66 -->
- [ ] F1-T08 — Implementar justificativa e reprocessamento de qualificação @Ethos [interno]  <!-- id:669a1d25-3e4c-49bd-9f4e-21eb2a580851 -->
- [ ] F1-T09 — Implementar tratamento de produto inativo/inexistente na qualificação @Ethos [interno]  <!-- id:84fb0c68-cbae-4426-8d36-9e0f4724da12 -->
- [ ] F1-T10 — Criar fila/modelo de transbordo @Ethos [interno]  <!-- id:d72db0a4-0f1a-4258-bdcd-da10af1d5d67 -->
- [ ] F1-T11 — Associar especialista ou pendência ao transbordo @Ethos [interno]  <!-- id:3bf990cf-99c5-40e9-8b8e-5a57d803c100 -->
- [ ] F1-T12 — Criar resolução/reabertura de transbordo @Ethos [interno]  <!-- id:9ec0c04b-6c7b-44ee-aa7d-0d25832d82ca -->
- [ ] F1-T13 — Gerar oportunidade a partir de lead qualificado @Ethos [interno]  <!-- id:a1335bed-ceac-4253-9c22-b75b89f31515 -->
- [ ] F1-T14 — Criar decisões aprovar/rejeitar/revisar @Ethos [interno]  <!-- id:9fbb5d58-f616-4390-bba9-de2bd060361b -->
- [ ] F1-T15 — Exigir motivo em rejeição e bloquear duplicidade @Ethos [interno]  <!-- id:6b5f5442-8335-499e-9f95-36d6ef6adf7d -->
- [ ] F1-T16 — Criar eventos/base de cálculo de métricas @Ethos [interno]  <!-- id:5c088f6e-32bc-44d4-9989-5f70447653e2 -->
- [ ] F1-T17 — Criar painel mínimo com filtros e estado sem dados @Ethos [interno]  <!-- id:06e48e47-b225-42b5-b312-8a0dfe640648 -->
- [ ] F1-T18 — Criar fixture de baseline com 10 leads @Ethos [interno]  <!-- id:85f366ec-9c92-4120-b9a3-50832a7b3b83 -->
- [ ] F1-T19 — Criar timeline de interações no lead @Ethos [interno]  <!-- id:af608224-bb6c-4ca6-831b-db0dbb37afeb -->
- [ ] F1-T20 — Implementar marcação de proposta e contagem K2 @Ethos [interno]  <!-- id:bc76cb44-ca46-43bb-9e7b-5c0b85184462 -->
- [ ] F1-T21 — Implementar cancelamento auditável de interação @Ethos [interno]  <!-- id:5b72a1fb-0bf9-4f74-8bad-7ef745bbf9c1 -->
