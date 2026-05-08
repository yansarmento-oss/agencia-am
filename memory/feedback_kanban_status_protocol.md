---
name: Kanban — Protocolo de movimentação de status
description: Quando mover o card de demanda entre colunas do Kanban (Yan precisa ver sinal visual do que estou fazendo)
type: feedback
originSessionId: e8c5e444-2033-47db-88e0-8846fe9f6dfe
---
Cada transição de trabalho exige mover o card de Demandas no Kanban como sinalização pro Yan.

**Sequência padrão:**
1. SM cria card → 📥 Demanda Criada
2. Eu gero estratégia → 🧠 Estratégia Gerada
3. Yan aprova → ✅ Estratégia Aprovada (ele move)
4. **Eu começo a escrever os 18 roteiros → mover IMEDIATAMENTE pra ✍️ Roteiros em Produção**
5. Terminei os roteiros → 👀 Revisão de Roteiros
6. Yan aprova tudo → 🎬 Prontos pra Gravar (ele move)
7. Publicados → 📤 Publicados (Cemitério)

**Campo de feedback** em cada fase:
- Bloco "💬 Feedback do aprovador" no final da seção de Estratégia (pra revisões de estratégia)
- Bloco "💬 Feedback dos roteiros" no final da seção de Roteiros (pra revisões de roteiro)
- Se precisa revisão, Yan mantém no status atual e escreve no feedback — eu detecto e refaço

**Why:** Yan precisa ver sinal visual do que está acontecendo. Card parado na coluna anterior enquanto trabalho = ansiedade. Card movido = tranquilidade.

**How to apply:** Em qualquer demanda, sempre mover ANTES de começar o trabalho (pra sinalizar "tô fazendo") e DEPOIS de terminar (pra sinalizar "tá pronto"). Cada seção longa deve ter seu próprio bloco de feedback.

**Padrão de resposta a feedback (estratégia E roteiros):**
Quando Yan deixa feedback pedindo revisões, após aplicar as mudanças, adicionar logo abaixo do feedback dele um bloco `### ✅ Checklist de revisão vN (respondendo ao feedback)` com cada pedido dele como item `- [x] ~~texto riscado~~ → o que foi feito`. Depois do checklist, um resumo "Mudanças da vN-1 → vN". Isso dá feedback visual claro de que todos os pedidos foram endereçados.
