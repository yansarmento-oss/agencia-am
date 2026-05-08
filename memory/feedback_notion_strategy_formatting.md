---
name: Notion — Formato padrão de estratégia de roteiros
description: Padrão visual que Yan definiu para documentos de estratégia mensal no Notion (agência médica)
type: feedback
originSessionId: e8c5e444-2033-47db-88e0-8846fe9f6dfe
---
Padrão visual obrigatório em TODO documento de estratégia de ciclo (18 posts) no Notion:

**Semanas:** heading3 toggle verde, sintaxe:
`###  **Semana N · dd/mm – dd/mm — tema** {toggle="true" color="green_bg"}`
(dois espaços após ###, conteúdo da semana indentado com tab dentro do toggle)

**Cada post:** callout indentado dentro do toggle da semana:
`<callout icon="EMOJI">` ... `</callout>`

**Emojis padronizados por formato:**
- 🎬 Reel (qualquer tipo: educativo, react, carismático, contexto, fechamento, emocional)
- 🎠 Carrossel
- 📸 Antes/Depois
- 🎥 Bastidor

**Why:** Yan quer visualização rápida — semanas colapsáveis em verde, posts destacados em amarelo (callouts default são amarelos). Padrão vale pros 16 clientes.

**How to apply:** Sempre que gerar estratégia de ciclo, usar essa estrutura. CRITICAL: usar caracteres reais (newlines e emojis), nunca escape sequences `\n` ou `\uXXXX` — senão Notion renderiza literal e quebra.
