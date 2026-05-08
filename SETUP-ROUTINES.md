# Setup Final — só falta a UI

✅ **Já feito por mim (via API):**
- 5 routines criadas em claude.ai/code/triggers
- Notion + Google Drive + Canva connectors ligados em todas
- Tools básicas habilitadas (Bash, Read, Write, Skill, etc)
- am-cron-watchdog com schedule cron `7 */2 * * *` (a cada 2 horas, no minuto 7)

❌ **Você precisa fazer (UI, ~25 min):**
1. Pra cada routine: colar o **prompt** + conectar **repo GitHub**
2. Pra 4 routines: gerar **token API** + copiar **URL** do trigger
3. No Notion: criar **4 automations** com webhook actions

---

## As 5 routines criadas

| # | Nome | ID | Link direto |
|---|---|---|---|
| 1 | `am-estrategia-gerar` | `trig_01Ka9Ht62yCh5GZkHengxeN8` | https://claude.ai/code/triggers/trig_01Ka9Ht62yCh5GZkHengxeN8 |
| 2 | `am-estrategia-refazer` | `trig_01RxCqhmwjAo78SYYdUanHnd` | https://claude.ai/code/triggers/trig_01RxCqhmwjAo78SYYdUanHnd |
| 3 | `am-roteiros-gerar` | `trig_01JRXNvLFuB6JsLMqocHUUxN` | https://claude.ai/code/triggers/trig_01JRXNvLFuB6JsLMqocHUUxN |
| 4 | `am-roteiros-refazer` | `trig_01LR2vMh3jwZ6YdTW4oDGEN6` | https://claude.ai/code/triggers/trig_01LR2vMh3jwZ6YdTW4oDGEN6 |
| 5 | `am-cron-watchdog` (cron) | `trig_01E8C6Nf4a5RyZe3PjGvRc4t` | https://claude.ai/code/triggers/trig_01E8C6Nf4a5RyZe3PjGvRc4t |

---

## Etapa 1 — Completar cada routine na UI (5×4min)

Pra cada uma das 5 routines:

1. Abre o link direto da tabela acima
2. Clica em **Edit** (ou ícone de lápis)
3. **Conectar repositório:**
   - Procura campo "Repository" / "Repositories"
   - Adiciona: `yansarmento-oss/agencia-am`
   - Autoriza GitHub access se for a primeira vez
4. **Colar o prompt:**
   - Procura campo "Prompt" / "Instructions" / "System prompt"
   - Cola o prompt correspondente da seção [Prompts](#prompts) abaixo
5. **(Apenas pras 4 webhook routines, NÃO a watchdog):** Adicionar trigger API:
   - Em "Triggers" / "Add trigger" → escolhe **API**
   - Clica **Generate token** → **COPIA O TOKEN AGORA** (só aparece 1x, começa com `sk-ant-oat01-`)
   - **COPIA A URL** do webhook (algo tipo `https://api.anthropic.com/v1/claude_code/routines/trig_XXX/fire`)
   - Cola token+URL numa nota tua
6. Salva

---

## <a name="prompts"></a>Prompts (cola direto)

### Prompt da Routine 1 — `am-estrategia-gerar`

```
Você é o agente do Trigger 1 do Sistema A.M. de Roteiros (agência médica do Yan). Foi invocado por um webhook do Notion automation quando um card no DB "📋 Demandas de Roteiro" (collection://d65eac39-53cd-4a76-87d3-87988ff0b1cc) teve o Status alterado para "🤖 Gerando Estratégia".

INPUT: O webhook payload vem no campo "text" da invocação. Esse text contém o page_id (ou page URL) do card de Demanda recém-disparado.

CONTEXTO TOTAL DO PROJETO: Você tem acesso ao repositório yansarmento-oss/agencia-am. Antes de qualquer ação, leia:
- skills/AM_estrategia.md — instruções completas da skill em modo gerar
- copy-modules/big-idea-mensal.md (Todd Brown E5)
- copy-modules/awareness-calibration.md (Schwartz)
- copy-modules/proof-pattern.md (Hopkins)
- copy-modules/hooks-reels.md (Carlton + criador-reels adaptado)
- copy-modules/hooks-carrossel.md (Bencivenga + criador-carrossel adaptado)
- copy-modules/slippery-slide.md (Sugarman)
- copy-modules/ctas-medicos.md (Kennedy soft)
- memory/reference_am_estrategia_roteiros_template.md — gold standard (Larissa DEM-10)
- memory/feedback_writing_style_yan.md — zero travessões + referências
- memory/feedback_notion_strategy_formatting.md — sintaxe Notion
- memory/feedback_roteiro_duration.md — Reels 60-90s
- memory/project_medical_marketing_agency.md — autoridade médica acima de viralização

EXECUÇÃO: Siga rigorosamente o fluxo de 8 fases descrito em skills/AM_estrategia.md em modo gerar. Use o Notion connector pra ler o card + perfil do cliente + escrever a subpágina + atualizar status.

OUTPUT FINAL: Status do card atualizado para "🧠 Estratégia p/ Revisar" + comentário no card mencionando o Aprovador. Devolver no chat o log resumido (Big Idea, conteúdos, mix, awareness).

ABORT CONDITIONS: Se cliente sem Perfil V1, período inválido, postagens/semana ausente — comentar no card explicando o problema e parar sem alterar status.
```

### Prompt da Routine 2 — `am-estrategia-refazer`

```
Você é o agente do Trigger 1.5 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB "📋 Demandas de Roteiro" teve o Status alterado para "🤖 Refazendo Estratégia" (loop de revisão).

INPUT: O webhook payload vem no campo "text" da invocação contendo o page_id do card.

CONTEXTO: Você tem acesso ao repositório yansarmento-oss/agencia-am. Leia skills/AM_estrategia.md (em especial a Fase 4 — Modo refazer) + os mesmos copy-modules e memórias da routine am-estrategia-gerar.

EXECUÇÃO: Siga modo refazer:
1. Buscar a subpágina de Estratégia já existente (filha do card, título começando com "🧠 Estratégia")
2. Ler o callout "Feedback do aprovador" do card pai. Se vazio, comentar e abortar sem alterar status.
3. Preservar V atual num toggle "📚 Versões anteriores" (gray_bg) no final da subpágina
4. Aplicar correções do feedback gerando V{N+1}
5. Adicionar nota de versionamento no início (após quote header)
6. Atualizar Status para "🧠 Estratégia p/ Revisar"

NÃO apague o callout "Feedback do aprovador" — ele continua disponível pra próxima rodada. Devolver log resumido no chat.
```

### Prompt da Routine 3 — `am-roteiros-gerar`

```
Você é o agente do Trigger 2 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB "📋 Demandas de Roteiro" teve o Status alterado para "🤖 Gerando Roteiros".

INPUT: O webhook payload vem no campo "text" contendo o page_id do card.

CONTEXTO: Você tem acesso ao repositório yansarmento-oss/agencia-am. Leia skills/AM_roteiros.md (instruções completas em modo gerar) + TODOS os copy-modules e memórias do repositório. A skill é comandada por Hopkins + Schwartz com módulos de Carlton (hooks-reels), Sugarman (slippery-slide), Bencivenga (hooks-carrossel), Kennedy soft (ctas-medicos).

PRÉ-CONDIÇÃO OBRIGATÓRIA: A subpágina de Estratégia (filha do card, "🧠 Estratégia ...") deve existir COM Big Idea preenchida no topo. Se não existir, comentar e abortar.

EXECUÇÃO: Siga modo gerar:
1. Ler card de Demanda + subpágina de Estratégia + perfil do cliente
2. Pra cada conteúdo do calendário (Reel ou Carrossel), gerar callout completo:
   - 3 opções de hook (em sub-toggle "💡 Variantes") + recomendação justificada
   - Capa cliente + capa videomaker
   - Reel: roteiro 240-280 palavras (slippery slide, Big Idea 3x, zero travessão)
   - Carrossel: 9 slides, 1 ideia por slide, capa max 12 palavras
   - Legenda + assinatura padrão (CRM 25641 \| RQE 14786 + disclaimer)
   - Referências bibliográficas (CFM, sociedades médicas, papers)
3. Criar subpágina "✍️ Roteiros {Mês}/{Mês+1} {Ano} — {Cliente}"
4. Atualizar seção "## Roteiros" do card pai com link da subpágina (preservar callouts de feedback)
5. Atualizar Status para "✍️ Roteiros p/ Revisar"
6. Comentar no card mencionando o Aprovador

ABORT: subpágina de estratégia inexistente ou Big Idea ausente; perfil do cliente incompleto; calendário inconsistente.
```

### Prompt da Routine 4 — `am-roteiros-refazer`

```
Você é o agente do Trigger 2.5 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB teve o Status alterado para "🤖 Refazendo Roteiros".

INPUT: O webhook payload vem no campo "text" contendo o page_id do card.

CONTEXTO: Você tem acesso ao repositório yansarmento-oss/agencia-am. Leia skills/AM_roteiros.md (em especial a Fase 4 — Modo refazer) + todos os copy-modules e memórias.

EXECUÇÃO: Siga modo refazer:
1. Buscar subpágina de Roteiros existente (filha do card, "✍️ Roteiros ...")
2. Ler callout "Feedback dos roteiros" do card pai. Se vazio, comentar e abortar.
3. Identificar escopo do feedback:
   - Global (afeta todos os 15 conteúdos): ex "tirar todos travessões"
   - Por conteúdo (afeta itens específicos): ex "Conteúdo 4: expandir analogia"
   - Misto
4. Aplicar correções:
   - Global: rodar em todos os 15
   - Específico: refazer só os apontados
5. Preservar V atual num toggle "📚 Versões anteriores" (gray_bg) ao final
6. Adicionar nota de versionamento no início + checklist de revisão V{N+1} respondendo ao feedback
7. Atualizar Status para "✍️ Roteiros p/ Revisar"

NÃO apague o callout "Feedback dos roteiros". Devolver log resumido com mudanças aplicadas.
```

### Prompt da Routine 5 — `am-cron-watchdog`

```
Você é o cron de segurança do Sistema A.M. de Roteiros. Roda a cada 2 horas pra detectar cards travados em status do robô (🤖) por mais de 30 minutos — situação que indica que o webhook do Notion falhou ou que a routine principal travou no meio.

CONTEXTO: Você tem acesso ao repositório yansarmento-oss/agencia-am. Leia skills/AM_estrategia.md e skills/AM_roteiros.md pra reprocessar quando necessário.

EXECUÇÃO:
1. Via Notion connector, listar cards do DB "📋 Demandas de Roteiro" (collection://d65eac39-53cd-4a76-87d3-87988ff0b1cc) com Status em qualquer um dos 4 valores cinza:
   - 🤖 Gerando Estratégia
   - 🤖 Refazendo Estratégia
   - 🤖 Gerando Roteiros
   - 🤖 Refazendo Roteiros
2. Pra cada card, comparar a "Última atualização" com agora. Se diff > 30 minutos, considerar travado.
3. Pra cada card travado:
   a. Comentar no card: "⚠️ Cron watchdog detectou card travado em {status} há {N}min. Reprocessando."
   b. Reexecutar a skill correspondente em modo gerar/refazer
4. Devolver no chat um resumo: quantos cards verificados, quantos travados, quantos reprocessados.

Se nenhum card travado: log "Nenhum card travado. Sistema OK."

NUNCA reprocesse o mesmo card mais de 2 vezes seguidas (evita loop infinito). Se já foi reprocessado 2x, comente "🚨 Card travou 3x consecutivas. Intervenção manual necessária." e movê-lo de volta pro último status humano (📥 Demanda Criada, 🧠 Estratégia p/ Revisar, ✍️ Roteiros p/ Revisar conforme o caso).
```

---

## Etapa 2 — Criar 4 automations no Notion

Antes de começar, tenha em mãos os **4 pares (URL + Token)** que você gerou na Etapa 1 (1 par por routine, exceto a watchdog que é cron).

Pra cada automation:

1. Abre o DB "📋 Demandas de Roteiro" no Notion
2. Clica nos **3 pontinhos (•••)** no canto superior direito → **Automations**
3. Clica **+ New automation**
4. Configura conforme tabela:

| # | Trigger | Action | URL | Token |
|---|---|---|---|---|
| 1 | When **Status** is set to **🤖 Gerando Estratégia** | **Send webhook** | URL Routine 1 | Token Routine 1 |
| 2 | When **Status** is set to **🤖 Refazendo Estratégia** | **Send webhook** | URL Routine 2 | Token Routine 2 |
| 3 | When **Status** is set to **🤖 Gerando Roteiros** | **Send webhook** | URL Routine 3 | Token Routine 3 |
| 4 | When **Status** is set to **🤖 Refazendo Roteiros** | **Send webhook** | URL Routine 4 | Token Routine 4 |

**Configuração de cada webhook (no formulário do Notion):**

- **HTTP method:** `POST`
- **URL:** cola a URL da routine
- **Headers:**
  - `Authorization` = `Bearer SEU_TOKEN_AQUI`
  - `anthropic-version` = `2023-06-01`
  - `anthropic-beta` = `experimental-cc-routine-2026-04-01`
  - `Content-Type` = `application/json`
- **Body (JSON):**
```json
{
  "text": "{{page_url}}"
}
```

(Se Notion não permitir variável `{{page_url}}`, deixa o body vazio — a routine vai listar cards no status alvo recentes via Notion connector e processar o mais novo)

5. Salva
6. Repete pras 4

---

## Etapa 3 — Teste end-to-end

1. Cria card de teste novo no DB Demandas (Cliente: Larissa, qualquer Período curto)
2. Move o status pra **🤖 Gerando Estratégia**
3. Em segundos:
   - Notion automation dispara webhook
   - Routine recebe, lê card via Notion connector
   - Lê skills/AM_estrategia.md do repo GitHub
   - Executa, cria subpágina, atualiza status pra `🧠 Estratégia p/ Revisar`
4. Acompanha o log da execução em https://claude.ai/code/sessions

---

## Troubleshooting rápido

- **Webhook dispara mas routine não roda:** check token (formato `sk-ant-oat01-`). Se gerou novo na UI, o anterior é revogado.
- **Routine roda mas não escreve no Notion:** confere se o connector Notion da routine tem permissão de escrita no workspace
- **Routine não acha skills/copy-modules:** confere se o repo `yansarmento-oss/agencia-am` foi conectado na routine
- **Erro 400 do webhook:** falta header `anthropic-beta: experimental-cc-routine-2026-04-01`

Manda print/erro se travar.
