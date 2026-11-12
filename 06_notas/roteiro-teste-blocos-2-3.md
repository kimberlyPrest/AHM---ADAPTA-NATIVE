# Roteiro de teste do owner — Blocos 2 e 3 (versão 0.0.54)

**Tempo estimado:** 15 minutos. **Onde:** preview do sistema, logado com o seu usuário.
**Ordem importa:** faça o Bloco 2 antes do Bloco 3 (o painel usa as interações do Bloco 2).

---

## Bloco 2 — Timeline de interações (insumo preparado pelo Bart)

O lead **Aroldo Teles da Silva (Stellantis)** já tem 3 interações: ligação (10/11), reunião (11/11) e proposta por e-mail (12/11).

1. **Leads** → localize o Aroldo → **Editar** → role até **Timeline de interações**.
2. Confira: **"Interações até a proposta: 3."**
3. **Registrar interação** → qualquer tipo, **mantenha a data de hoje** → Registrar. A contagem continua **3** (a proposta já fechou).
4. Na interação da proposta → **Cancelar interação** → informe um motivo → Confirmar. A contagem vira **"Sem proposta registrada — 3 interações até agora."** (a cancelada não conta).
5. Em outra interação → **Marcar proposta** → descreva o que foi proposto → Confirmar. A contagem volta a aparecer.
6. Tente **Marcar proposta** em mais uma interação → deve ser **bloqueado** com a mensagem "Este lead já tem uma proposta registrada em outra interação…".

**Aprovado se:** os números batem, os selos "Proposta" e "Cancelada" aparecem e a interação cancelada continua visível com o motivo.

---

## Bloco 3 — Painel de métricas (nada a preparar — a demonstração é carregada por você)

1. Menu lateral → **Métricas**.
2. Com os **dados reais** (fonte "Todas as fontes"):
   - **Qualificação automática** deve aparecer **0%** ou **"—"**. Isso é esperado: hoje toda qualificação é feita por clique (manual). É exatamente o "antes" que a qualificação automática vai mudar.
   - **Interações até a proposta** mostra o resultado do Aroldo (o valor depende do que você fez no Bloco 2).
   - **Capacidade por vendedor** mostra os leads processados por responsável.
3. Clique **Carregar demonstração** → aviso "10 leads de demonstração criados".
4. Troque a fonte para **Demonstração** e confira:
   - **Qualificação automática: 70%** (7 de 10 leads do recorte piloto).
   - **Interações até a proposta: 4** (média de 3 leads com proposta).
   - **Capacidade por vendedor: 10**, no seu nome.
5. Passe pelos períodos (**7 / 30 / 90 dias / Todo o período**) — nenhum erro; textos de "dados insuficientes" claros quando não houver dados.
6. **Copiar resumo** → cole num bloco de notas: os três indicadores em texto.
7. Confira nas outras telas que a demonstração aparece identificada como **"DEMO"**: Leads (10), Oportunidades (7 sugeridas) e Transbordos (1).
8. Volte em **Métricas** → **Remover demonstração** → confirme. Os itens "DEMO" somem de Leads, Oportunidades e Transbordos; os dados reais continuam.

**Aprovado se:** os números da demonstração batem (70% / 4 / 10), a remoção limpa tudo e nenhum texto mostra código interno.

---

## Pontos de atenção para o Bart (provas antes do teste do owner)

- A rota de capacidade (`/backend/v1/metricas/capacidade`) foi verificada apenas sem login (responde 401 como esperado). **Provar com usuário autenticado** os quatro valores de fonte e os quatro períodos, e um valor inválido (deve responder 400).
- **Carregar e remover a demonstração** uma vez antes do owner, confirmando 70% / 4 / 10 e a limpeza completa (inclusive oportunidades, transbordo e interações DEMO).
- Os testes automáticos (24) passaram fora do Skip; o QA do Skip **não executa** a suíte (`test: ran false`).
