# Conjunto de referência — pré-qualificação

É a "prova oficial" da inteligência do sistema: casos reais com a resposta que **o owner** daria. Toda mudança nas regras ou no agente de IA é medida contra este conjunto ("acertou 46 de 50"). Ver emenda 1.3a, §5.

## Como preencher

1. Abra `casos.csv` no Excel (separador ponto e vírgula).
2. Uma linha por caso real do histórico — de 30 a 50 casos. Os 10 primeiros alimentam a prova técnica da IA (Fatia 0).
3. Apague as 2 linhas de exemplo (`EX-01`, `EX-02`) — elas são fictícias e servem só de modelo.
4. **Anonimize** nome e contato da pessoa (ex.: "Contato 07"). Empresa e segmento podem ficar.

## Equilíbrio sugerido

| Tipo de caso | Quantidade aproximada |
|---|---|
| Qualificado (cliente ideal, produto claro) | 40% |
| Pendente (faltam informações) | 25% |
| Transbordo (dúvida técnica, solução especial) | 20% |
| Fora do recorte (alta complexidade, sem aderência) | 15% |

Inclua de propósito casos difíceis: texto vago, produto citado errado, mais de um produto possível, cliente internacional, mensagem curta de WhatsApp.

## Colunas

| Coluna | O que colocar |
|---|---|
| `id_caso` | Código sequencial (C-01, C-02…) |
| `canal` | Site · Landing page · WhatsApp · E-mail · Indicação · Feira · Outro |
| `texto_original` | A mensagem ou formulário **exatamente como o cliente escreveu** (sem corrigir) |
| `empresa` / `segmento` / `pais` | Dados da empresa |
| `area_interesse_esperada` | Qualidade no Transporte · Cadeia do Frio · Segurança e Logística |
| `produto_esperado` | Nome do produto como está no catálogo (vazio se não há produto) |
| `familia_esperada` | Família de diagnóstico |
| `modelo_negocio_esperado` | Cíclico · Assinatura · Locação · Pontual · Consultoria de Serviços |
| `urgencia_esperada` | Baixa · Média · Alta |
| `status_esperado` | qualificado · pendente · fora do recorte · transbordo |
| `perguntas_faltantes_esperadas` | O que você perguntaria em seguida (separar por " / ") |
| `motivo_transbordo` | Por que precisa de especialista (só se transbordo) |
| `resultado_real` | O que aconteceu de fato: Proposta enviada · Ganho · Perdido · Sem retorno |
| `observacao_owner` | Qualquer raciocínio seu que a IA deveria aprender |
