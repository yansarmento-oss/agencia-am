# Setup das Routines + Automations Notion

Este documento te guia em **2 etapas** pra ligar o sistema:

1. Criar 5 routines em [claude.ai/code/routines](https://claude.ai/code/routines) (cada uma com URL pública pro Notion chamar)
2. Criar 4 automations no Notion DB "📋 Demandas de Roteiro"

Tempo total: ~30 minutos.

---

## Etapa 1 — Criar as 5 routines em claude.ai/code/routines

Pra cada routine abaixo, faz isso:

1. Vai em https://claude.ai/code/routines
2. Clica em **+ New routine** (ou similar)
3. Em **Name**: cola o nome exato (ex: `am-estrategia-gerar`)
4. Em **Repository**: conecta `yansarmento-oss/agencia-am` (autoriza GitHub access se for a primeira vez)
5. Em **Prompt**: cola o prompt completo da seção correspondente abaixo
6. Em **Triggers**: adiciona um trigger do tipo **API** → clica **Generate token** → **copia o token e a URL imediatamente** (token só aparece 1x)
7. Salva a routine
8. Cola o token e URL numa anotação tua (vai precisar pra Etapa 2)

### Routine 1 — `am-estrategia-gerar`

**Name:** `am-estrategia-gerar`

**Repository:** `yansarmento-oss/agencia-am`

**Prompt:**
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

---

### Routine 2 — `am-estrategia-refazer`

**Name:** `am-estrategia-refazer`

**Repository:** `yansarmento-oss/agencia-am`

**Prompt:**
```
Você é o agente do Trigger 1.5 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB "📋 Demandas de Roteiro" teve o Status alterado para "🤖 Refazendo Estratégia" (loop de revisão).

INPUT: O webhook payload vem no campo "text" da invocação contendo o page_id do card.

CONTEXTO: Leia skills/AM_estrategia.md (em especial a Fase 4 — Modo refazer) + os mesmos copy-modules e memórias da routine am-estrategia-gerar.

EXECUÇÃO: Siga modo refazer:
1. Buscar a subpágina de Estratégia já existente (filha do card, título começando com "🧠 Estratégia")
2. Ler o callout "Feedback do aprovador" do card pai. Se vazio, comentar e abortar sem alterar status.
3. Preservar V atual num toggle "📚 Versões anteriores" (gray_bg) no final da subpágina
4. Aplicar correções do feedback gerando V{N+1}
5. Adicionar nota de versionamento no início (após quote header)
6. Atualizar Status para "🧠 Estratégia p/ Revisar"

NÃO apague o callout "Feedback do aprovador" — ele continua disponível pra próxima rodada. Devolver log resumido no chat.
```

---

### Routine 3 — `am-roteiros-gerar`

**Name:** `am-roteiros-gerar`

**Repository:** `yansarmento-oss/agencia-am`

**Prompt:**
```
Você é o agente do Trigger 2 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB "📋 Demandas de Roteiro" teve o Status alterado para "🤖 Gerando Roteiros".

INPUT: O webhook payload vem no campo "text" contendo o page_id do card.

CONTEXTO: Leia skills/AM_roteiros.md (instruções completas em modo gerar) + TODOS os copy-modules e memórias do repositório. A skill é comandada por Hopkins + Schwartz com módulos de Carlton (hooks-reels), Sugarman (slippery-slide), Bencivenga (hooks-carrossel), Kennedy soft (ctas-medicos).

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

---

### Routine 4 — `am-roteiros-refazer`

**Name:** `am-roteiros-refazer`

**Repository:** `yansarmento-oss/agencia-am`

**Prompt:**
```
Você é o agente do Trigger 2.5 do Sistema A.M. de Roteiros. Foi invocado por um webhook do Notion automation quando um card no DB teve o Status alterado para "🤖 Refazendo Roteiros".

INPUT: O webhook payload vem no campo "text" contendo o page_id do card.

CONTEXTO: Leia skills/AM_roteiros.md (em especial a Fase 4 — Modo refazer) + todos os copy-modules e memórias.

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

---

### Routine 5 — `am-cron-watchdog` (cron de segurança)

**Name:** `am-cron-watchdog`

**Repository:** `yansarmento-oss/agencia-am`

**Schedule (cron):** `7 */2 * * *` (a cada 2 horas, no minuto 7)

**Prompt:**
```
Você é o cron de segurança do Sistema A.M. de Roteiros. Roda a cada 2 horas pra detectar cards travados em status do robô (🤖) por mais de 30 minutos — situação que indica que o webhook do Notion falhou ou que a routine principal travou no meio.

EXECUÇÃO:
1. Via Notion connector, listar cards do DB "📋 Demandas de Roteiro" (collection://d65eac39-53cd-4a76-87d3-87988ff0b1cc) com Status em qualquer um dos 4 valores cinza:
   - 🤖 Gerando Estratégia
   - 🤖 Refazendo Estratégia
   - 🤖 Gerando Roteiros
   - 🤖 Refazendo Roteiros
2. Pra cada card, comparar a "Última atualização" com agora. Se diff > 30 minutos, considerar travado.
3. Pra cada card travado:
   a. Comentar no card: "⚠️ Cron watchdog detectou card travado em {status} há {N}min. Reprocessando."
   b. Reexecutar a skill correspondente em modo gerar/refazer (ler skills/AM_estrategia.md ou skills/AM_roteiros.md conforme o status)
4. Devolver no chat um resumo: quantos cards verificados, quantos travados, quantos reprocessados.

Se nenhum card travado: log "Nenhum card travado. Sistema OK."

NUNCA reprocesse o mesmo card mais de 2 vezes seguidas (evita loop infinito). Se já foi reprocessado 2x, comente "🚨 Card travou 3x consecutivas. Intervenção manual necessária." e movê-lo de volta pro último status humano (📥 Demanda Criada, 🧠 Estratégia p/ Revisar, ✍️ Roteiros p/ Revisar conforme o caso).
```

**OBSERVAÇÃO:** Pra essa routine, NÃO precisa adicionar trigger API. Ela já dispara sozinha pelo schedule cron.

---

## Etapa 2 — Criar 4 automations no Notion

Antes de começar, tenha em mãos os 4 pares (URL + Token) das routines 1-4 que você gerou na Etapa 1.

Pra cada automation:

1. Abre o DB "📋 Demandas de Roteiro" no Notion
2. Clica nos **3 pontinhos (•••)** no canto superior direito do DB → **Automations**
3. Clica **+ New automation**
4. Configura conforme a tabela abaixo

### Tabela das 4 automations

| # | Trigger | Action | URL (sua, da Routine X) | Token (Bearer) |
|---|---|---|---|---|
| 1 | When **Status** is set to **🤖 Gerando Estratégia** | **Send webhook** | URL da Routine 1 | Token da Routine 1 |
| 2 | When **Status** is set to **🤖 Refazendo Estratégia** | **Send webhook** | URL da Routine 2 | Token da Routine 2 |
| 3 | When **Status** is set to **🤖 Gerando Roteiros** | **Send webhook** | URL da Routine 3 | Token da Routine 3 |
| 4 | When **Status** is set to **🤖 Refazendo Roteiros** | **Send webhook** | URL da Routine 4 | Token da Routine 4 |

### Configuração detalhada de cada webhook

Quando o Notion pedir os parâmetros do webhook:

- **HTTP method:** `POST`
- **URL:** cola a URL da routine correspondente
- **Headers:**
  - `Authorization`: `Bearer SEU_TOKEN_AQUI` (substitui SEU_TOKEN_AQUI pelo token da routine)
  - `anthropic-version`: `2023-06-01`
  - `anthropic-beta`: `experimental-cc-routine-2026-04-01`
  - `Content-Type`: `application/json`
- **Body (JSON):**
```json
{
  "text": "{{page_url}}"
}
```

(Algumas UIs do Notion permitem variáveis tipo `{{page_url}}` ou `{{page_id}}`. Se não permitir variável, pode ficar em branco — a routine vai conseguir identificar o card pela mudança de status disparada e listar cards recentes do status alvo.)

5. Salva a automation
6. Repete pras 4 transições

---

## Etapa 3 — Teste end-to-end

1. Cria um card de teste novo no DB Demandas
2. Preenche: Cliente (Larissa), Período (qualquer), Postagens/semana, Orientações
3. Move o status pra **🤖 Gerando Estratégia**
4. Em segundos, deve aparecer:
   - Comentário no card "iniciando..."
   - Subpágina sendo criada
   - Status mudando pra **🧠 Estratégia p/ Revisar**
5. Se travar, conferir em https://claude.ai/code/sessions o log da routine

---

## Troubleshooting

- **Webhook não dispara:** verificar se o Notion plan tem webhook actions habilitado (Plus+ tem). Conferir headers e body.
- **Routine recebe webhook mas não acessa DB:** confirmar que o Notion connector está habilitado na routine + autorizou acesso ao workspace.
- **Routine recebe mas não escreve:** verificar permissões de escrita do connector Notion na sua workspace.
- **Token rejeitado:** token de routine começa com `sk-ant-oat01-`. Se gerou novo, o anterior é revogado.

---

Conforme você for configurando, manda print/erro pra eu desbloquear.
