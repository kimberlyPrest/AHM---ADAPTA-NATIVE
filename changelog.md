# Changelog

## 2026-09-17

- Pasta operacional inicial criada para a Fase 1.

- 2026-11-09 · [Afonso/Rose] · Task F1-T01 concluída: modelo e validações do catálogo de produtos (migration 0007_create_produtos + hooks RN-1.1/CA-1-002). Provas reexecutadas via API em 09/11: produto completo Ativo → 200; incompleto tentando Ativo → 400; campos obrigatórios ausentes → 400 por campo. Teste humano aprovado pelo owner Afonso ("✅ Aprovo o teste da F1-T01").

- 2026-11-09 · [Afonso/Rose] · Task F1-T02 concluída: tela/lista de catálogo com ativar/inativar (src/pages/Produtos.tsx, rota /produtos, navegação no Layout). Skip 0.0.5 (hash 7f46b2a), QA completo OK. Provas reexecutadas via API em 09/11: produto ativo aparece no filtro Ativo; inativado some da seleção (lista vazia); reativado volta. Ciclo verificado também na tela do preview. Teste humano aprovado pelo owner Afonso ("✅ Testei e funcionou").
