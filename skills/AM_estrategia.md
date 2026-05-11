# am-estrategia

Geração (ou re-geração com feedback) da subpágina **Estratégia** dentro de um card de Demanda no Sistema A.M. de Roteiros. Disparada por mudança de status no DB Demandas. Produz calendário completo do ciclo (datas-chave + pilares + territórios + N posts em callouts dentro de toggles semanais), seguindo o template canônico (Dra. Larissa DEM-10).

---

## Contexto (ler antes de executar)

- DB Demandas: data source `collection://d65eac39-53cd-4a76-87d3-87988ff0b1cc`
- DB Clientes: data source `collection://9a66bcde-df8a-4286-bd25-e7e592660e11`
- Parent page: "A.M. - Sistema de Roteiros" (`341627656baf8085a6f8fa2c18a1aad8`)
- Memórias **obrigatórias** de ler antes:
  - `reference_am_estrategia_roteiros_template.md` — template canônico (estrutura, taxonomia, regras CFM)
  - `feedback_am_kanban_visual_convention.md` — semântica de cada status (cinza = robô, colorido = humano)
  - `feedback_writing_style_yan.md` — zero travessões + referências
  - `feedback_notion_strategy_formatting.md` — sintaxe Notion (toggle verde, callout, emojis por formato)
  - `feedback_roteiro_duration.md` — Reels 60-90s
  - `project_medical_marketing_agency.md` — autoridade médica acima de viralização
- Copy modules **obrigatórios** de ler em `/Users/Usuario/agencia-am/copy-modules/`:
  - `big-idea-mensal.md` — Todd Brown E5 Method, tese central do ciclo (Fase 0)
  - `awareness-calibration.md` — Eugene Schwartz, calibração por nível de consciência
  - `proof-pattern.md` — Claude Hopkins, hierarquia de fontes científicas
  - `hooks-reels.md` — John Carlton + criador-reels adaptado, 4 categorias de hook
  - `hooks-carrossel.md` — Gary Bencivenga + criador-carrossel adaptado, 4 categorias de capa
  - `ctas-medicos.md` — Dan Kennedy soft, banco de CTAs por intenção

## Input esperado

A skill recebe (via RemoteTrigger payload do webhook Notion, ou invocação manual):

- **page_id** ou **page_url** — card de Demanda no DB Demandas
- (opcional) **modo** — `gerar` ou `refazer`. Se omitido, detecta pelo status atual.

## Detecção de modo pelo status

Lê `Status` do card:

- `🤖 Gerando Estratégia` → modo **gerar** (primeira versão)
- `🤖 Refazendo Estratégia` → modo **refazer** (loop com feedback)
- Qualquer outro status → **abort**, comentar no card "Skill disparada com status incorreto: {status}. Esperado: 🤖 Gerando ou 🤖 Refazendo Estratégia." e parar.

## Fluxo de execução

### Fase 1 — Coleta de dados

1. **Fetch do card de Demanda** (mcp__claude_ai_Notion__notion-fetch). Extrair:
   - `Demanda` (título), `Demanda ID`
   - `Cliente` (relation → URL da página do cliente)
   - `Período` (start, end)
   - `Postagens por semana` (number)
   - `Orientações do mês` (texto livre — direções do SM, inputs do médico, temas pedidos)
   - `Referências/Links` (URL opcional)
   - `Aprovador` (people)
   - `Status` atual (pra detectar modo)

2. **Validar inputs obrigatórios:**
   - Cliente preenchido (relation não vazia)
   - Período (start + end) preenchido
   - Postagens por semana > 0

   Se faltar qualquer um: comentar no card listando os campos vazios + abort sem alterar status.

3. **Fetch do card de Cliente** (relation). Extrair TODOS os campos relevantes:
   - **Identidade:** Nome, Especialidade, Subnicho, Localização, RQE, Assinatura padrão, Tagline
   - **Voz:** Tom de voz, Arquétipo de marca, Bordões, Analogias, Hooks favoritos, CTAs padrão
   - **Posicionamento:** Público-alvo, Nível de consciência, Mix autoridade/conversão, Produto-âncora
   - **Governança:** Proibições (multi-select), Temas proibidos
   - **Visual (apenas referência):** Cores primárias, Cores secundárias, Tipografia, Mood visual
   - **Conteúdo do Perfil V1** (corpo da página) — pilares de conteúdo sugeridos, posicionamento detalhado

   Se cliente não tem Perfil V1 ou campos críticos vazios (Nome, Especialidade, Tom de voz, Hooks favoritos, CTAs padrão, Proibições): comentar no card "Perfil do cliente {Nome} incompleto. Campos faltando: [lista]. Trigger 0 (onboarding) deveria ter rodado antes." + abort.

### Fase 2 — Big Idea do ciclo (Todd Brown E5)

**Antes de calendarizar, define a tese central do mês.** Este é o passo mais importante. 15 conteúdos sem Big Idea são lista. Com Big Idea são campanha.

Seguir `copy-modules/big-idea-mensal.md` rigorosamente. Output desta fase é um bloco com 4 elementos:

1. **Big Idea (E1)** — frase de 6-12 palavras que contraria crença popular e defende verdade clínica do médico. Estrutura: "{O que público acha que é} **não é**. {O que realmente é}."
2. **Mecanismo único do médico (E2)** — extraído do cruzamento entre Subnicho + Bordões + Analogias + Tom + Produto-âncora do Perfil V1.
3. **Promessa de método (E3)** — não promete resultado (CFM). Promete avaliação, plano individual, conformidade, escuta, tempo de decisão.
4. **CTA do mês (E5)** — ação única que todos os 15 conteúdos convergem (geralmente "agendar avaliação" via canal específico).

**Validação obrigatória** antes de avançar (ver checklist em `big-idea-mensal.md`):
- Big Idea cabe em 6-12 palavras
- Contraria crença popular específica
- Defende verdade clínica do médico (não é slogan vazio)
- Coerente com Tom + Bordões + Analogias do Perfil V1
- Não promete resultado
- Mecanismo único é específico (não "anos de experiência")
- CTA do mês é único e repetível em variações

Se algum item falhar: refazer Big Idea antes de calendarizar. **Não calendariza sem Big Idea sólida.**

### Fase 3 — Cálculos do ciclo

1. **Número de semanas:** calcular do Período (`end - start` em dias / 7, arredondar pra cima).
2. **Total de conteúdos:** `semanas × postagens/semana`.
3. **Mix:** parsear o campo "Mix autoridade/conversão" do cliente (ex: "70/20/10" → Autoridade 70% / Conversão 20% / Conexão 10%). Se ausente, usar default `60/30/10` e marcar como assumption no callout final.
4. **Distribuição alvo:** calcular N de cada camada (ex: 15 posts × 60% = 9 Autoridade, 5 Conversão, 1 Conexão; arredondar pra somar exato).
5. **Distribuição por nível de consciência:** ler `Nível de consciência` do Perfil V1 (predominante do público). Distribuir os 15 conteúdos pelos 5 níveis (Unaware → Most Aware) seguindo `awareness-calibration.md`. Default sugerido se cliente é "médio": 25% Unaware, 30% Problem Aware, 25% Solution Aware, 12% Product Aware, 8% Most Aware. Ajustar conforme nível dominante do cliente.
6. **Datas-chave do período:** identificar todas as datas comemorativas relevantes pra especialidade do cliente que caem dentro do Período. Sempre considerar:
   - Datas universais (Dia das Mães, Dia dos Pais, Natal, Ano Novo, etc)
   - Datas da especialidade (Dia do Oftalmologista 7/5, Dia do Cardiologista 14/8, Dia da Mulher 8/3 pra ginecologia, Dia Mundial do Diabetes 14/11 pra endocrinologia, etc)
   - Datas regionais relevantes à Localização
7. **Distribuição por pilar:** mapear pilares do Perfil V1 do cliente. Garantir cobertura balanceada — nenhum pilar com 0 conteúdos no ciclo, sem repetir o mesmo pilar 2x na mesma semana sem justificativa.
8. **Distribuição por relação com a Big Idea:** cada conteúdo precisa ter relação clara com a Big Idea (Defesa direta / Ilustração / Prova / Desmontagem / Aplicação / Convite). Distribuição típica: 1-2 Defesa, 4-5 Ilustração, 3-4 Prova, 2-3 Desmontagem, 2-3 Aplicação, 1-2 Convite.

### Fase 4 — Construção do calendário

Para cada semana do Período:

1. Definir **tema da semana** (frase curta sintetizando narrativa) considerando:
   - Datas-chave que caem na semana
   - Progressão temática (semana 1 = abertura/institucional, semana última = encerramento de ciclo, intermediárias = densidade técnica)
   - Aderência à Big Idea do mês
2. Para cada conteúdo da semana, definir:
   - **Formato** (Reel ou Carrossel) — distribuir conforme padrão da agência (default: 70% Reels, 30% Carrosséis; Carrossel pra conteúdo educativo denso ou listas)
   - **Camada** (Autoridade · Conversão · Conexão)
   - **Nível de consciência** alvo (Unaware → Most Aware) — ver `awareness-calibration.md`
   - **Relação com a Big Idea** (Defesa direta / Ilustração / Prova / Desmontagem / Aplicação / Convite)
   - **Eixo** — Pilar OU Território OU Subnicho OU Sazonal (pode cruzar 2)
   - **Hook** — categoria preferida conforme nível de consciência (ver `hooks-reels.md` pra Reel ou `hooks-carrossel.md` pra Carrossel). Não escrever hook final aqui (isso é trabalho da `/AM_roteiros`); definir apenas o ângulo do hook + categoria sugerida.
   - **Copy** (Reel) — parágrafo de 3-5 linhas descrevendo conteúdo. Voz do médico, tom do cliente.
   - **Estrutura sugerida** (Carrossel) — lista numerada de 9 slides: capa → 7 desenvolvimento → CTA
   - **CTA** — escolher do banco "CTAs padrão" do cliente conforme `ctas-medicos.md`. Pra peças didáticas/institucionais usar "Sem CTA direto, peça didático" ou "Sem CTA, peça de marca" conforme couber.

### Fase 5 — Validações operacionais (auto-checagem antes de escrever)

Antes de gerar a subpágina, rodar checks:

1. **Big Idea cumprida?** Todos os 15 conteúdos têm relação clara com a Big Idea (defesa, ilustração, prova, desmontagem, aplicação ou convite)?
2. **Mix bate?** Contagem por camada coincide com alvo (margem ±1 conteúdo).
3. **Distribuição por nível de consciência OK?** Todos os 5 níveis cobertos, com peso ajustado ao Nível de Consciência predominante do cliente.
4. **Proibições respeitadas?** Nenhum hook/copy/CTA viola as Proibições do cliente (humor, dancinha, antes/depois, preço, urgência artificial, ataque a concorrente, promessa de resultado).
5. **Conformidade CFM?** Verificar Res. CFM 2.336/2023 — sem promessa de resultado, sem comparação entre pacientes, sem banalização cirúrgica, sem auto-diagnóstico induzido sem disclaimer.
6. **Distribuição por pilar OK?** Todos pilares cobertos, sem repetição forçada.
7. **Zero travessão** em todo texto gerado.
8. **Hooks coerentes com nível de consciência?** Categoria de hook sugerida bate com awareness alvo (ver `awareness-calibration.md` tabela rápida).
9. **CTAs do banco do cliente?** Nenhum CTA inventado fora do banco sem justificativa.

Se algum check falhar: corrigir antes de escrever. Se não conseguir corrigir sem comprometer qualidade: comentar no card explicando o trade-off + escrever a versão melhor possível + flagar nas validações operacionais finais.

### Fase 6 — Escrita no Notion

#### 🚨 REGRA DE OURO — Notion-flavored Markdown (LEIA ANTES DE QUALQUER tool call)

Toda chamada a `notion-create-pages` (`content`) ou `notion-update-page` (`content_updates`, `new_str`) tem que receber **caracteres reais**, não strings com escape literal. Esse é o bug #1 das routines e quebra o card por inteiro.

**❌ ERRADO** (resultado renderiza texto cru tipo "Estratégian\<callout\>nt..."):
- Passar `"## Estratégia\\n<callout icon=\"📝\">"` no JSON (escape duplo do backslash → backslash literal no Notion)
- Passar `"## Estratégian<calloutn"` (engoliu o backslash do `\n`, letra "n" sobrou)
- Escapar `<` e `>` com `\<` e `\>` (Notion não exige escape de tags, e o escape vira texto literal)

**✅ CERTO** (resultado renderiza bloco real):
- No JSON do tool call, usar `"\n"` (uma barra + n) → Notion recebe newline real
- Usar `<callout icon="📝">` e `</callout>` SEM escape de `<` e `>`
- Pipe em `CRM 12345 | RQE 678` precisa ser escapado como `\|` só dentro de tabelas/inline markdown — em callouts comuns, pipe normal funciona
- Emojis literais (📝 🎬 🎠 🧠) — nunca escape Unicode `\uXXXX`

**Antes de chamar a tool, mentalmente conte:** se a string contém `\\n` (dois backslashes) ou `\<` (backslash + <), tá errado. Re-escreva com newline real e tags sem escape.

#### Modo `gerar`

**Passo 1 — Criar subpágina de Estratégia (filha do card):**

Use `mcp__claude_ai_Notion__notion-create-pages` com:
- `parent`: `{ "type": "page_id", "page_id": "<UUID do card>" }`
- `properties`: `{ "title": "🧠 Estratégia {MesIniAbrev}/{MesFimAbrev} {Ano} — {NomeCliente}" }`
- `content`: corpo completo da estratégia (Big Idea + datas-chave + pilares + territórios + toggles por semana com callouts dos N conteúdos + validações operacionais + próximo passo). Ver template em `reference_am_estrategia_roteiros_template.md`.

Anote o `page_url` retornado — será usado no Passo 2.

**Passo 2 — Atualizar a seção `## Estratégia` do card pai (cirúrgico):**

Primeiro, fetch do card pai pra obter o conteúdo atual (`notion-fetch` com `id: page_url do card`).

Olhe o `<content>` retornado. Há 2 cenários:

**Cenário A — card pai JÁ tem as 4 seções no formato correto** (4 headings: `## Estratégia` + `## Feedback do aprovador` + `## Roteiros` + `## Feedback dos roteiros`):

Use `notion-update-page` com `command: "update_content"` e UM ÚNICO `content_updates`:

```json
{
  "old_str": "## Estratégia\n[CONTEÚDO ATUAL DA SEÇÃO — copie LITERAL do fetch, do heading até logo antes do próximo heading]",
  "new_str": "## Estratégia\n<page url=\"{URL_SUBPAGINA_CRIADA}\">🧠 Estratégia {Mes}/{Mes+1} {Ano} — {NomeCliente}</page>"
}
```

**CRITICAL:** o `old_str` precisa bater EXATO com o que o fetch retornou (incluindo eventuais `\<page\>` quebrados se já houver versão anterior). Não invente; copie literal. NÃO use `replace_content` — vai apagar os callouts de feedback.

**Cenário B — card pai NÃO tem as 4 seções** (estrutura ausente, ou só placeholder vazio do template):

Use `notion-update-page` com `command: "replace_content"` e `new_str` contendo o corpo completo das 4 seções. **Atenção máxima à regra de ouro acima** — newlines reais, tags `<callout>` sem escape:

```
## Estratégia
<page url="{URL_SUBPAGINA_CRIADA}">🧠 Estratégia {Mes}/{Mes+1} {Ano} — {NomeCliente}</page>

## Feedback do aprovador
<callout icon="📝">
	Espaço para anotações do aprovador sobre a estratégia. Aprovar, ajustar ou refazer? Quais conteúdos mudam, quais ficam, quais somem?
</callout>

## Roteiros
*Subpágina de roteiros será criada automaticamente pela `/AM_roteiros` quando o card for movido para `✅ Estratégia Aprovada`.*

## Feedback dos roteiros
<callout icon="📝">
	Espaço para anotações sobre os roteiros após produção (ciclo seguinte do Kanban).
</callout>
```

#### Modo `refazer`

1. Buscar a subpágina de Estratégia já existente (filha do card, título começando com `🧠 Estratégia`).
2. Ler o callout "Feedback do aprovador" do card pai. Se vazio: comentar no card "Status `🤖 Refazendo Estratégia` mas callout de feedback vazio. Não há orientações pra refazer." + abort sem alterar status.
3. Ler conteúdo atual da subpágina (V1 ou versão mais recente).
4. **Preservar histórico:** dentro da subpágina, criar (ou atualizar) um toggle no FINAL chamado `📚 Versões anteriores` (color: gray_bg). Mover o conteúdo da V atual pra dentro desse toggle, prefixado por `### V{N} · gerada em {data ISO}` (incrementar N).
5. Aplicar correções do feedback gerando V{N+1}. Reescrever o corpo principal da subpágina com a nova versão. Use `notion-update-page` `replace_content` na **subpágina** (não no card pai) — o card pai não deve ser tocado em modo refazer (já está com link da subpágina correto, callouts de feedback intactos).
6. Adicionar nota no início da subpágina (após o quote header): `> **Versão {N+1}** · refeita em {data ISO} aplicando feedback do aprovador. Histórico no toggle "📚 Versões anteriores" no final desta página.`
7. **NÃO** tocar no card pai. NÃO sobrescrever os callouts de feedback.

#### Detalhes técnicos de renderização Notion (lembrete final)

- Heading toggle: `### **Título** {toggle="true" color="green_bg"}` — atributo entre chaves no FINAL do heading
- Conteúdo do toggle: indentar 1 tab abaixo do heading
- Callout: `<callout icon="EMOJI">` ... `</callout>` — tags em linhas separadas, conteúdo indentado 1 tab dentro
- Newlines: caracteres reais (apertando Enter no editor). No JSON do tool call, `"\n"` (uma barra) → Notion recebe newline real.
- Emojis: caracteres literais (📝 🎬 🎠 🧠) — nunca `\uXXXX`
- Pipe em assinaturas dentro de tabela ou inline: `CRM 25641 \| RQE 14786`. Fora de tabela, pipe normal.
- `<page url="...">texto</page>` pra referenciar página filha (subpágina de estratégia)

### Fase 7 — Atualização de status

1. Atualizar `Status` do card pai pra `🧠 Estratégia p/ Revisar`.
2. Comentar no card mencionando o `Aprovador` (se houver): `Estratégia {gerada/refeita} e pronta pra revisão. Big Idea: "{tese curta}". {N} conteúdos no ciclo. Mix final: X/Y/Z.`
3. **NÃO** apagar o callout "Feedback do aprovador" — mesmo no modo refazer. Ele continua disponível pra próxima rodada.

### Fase 8 — Output final (log)

Em modo invocação manual, devolver:

```
✅ Estratégia {gerada/refeita V{N+1}} — {NomeCliente} {MesIni}/{MesFim} {Ano}
Big Idea: "{tese de 6-12 palavras}"
Card: {URL}
Subpágina: {URL}
Conteúdos: {N} ({X Reels} + {Y Carrosséis})
Mix final: {A}/{B}/{C} (alvo {Mix do cliente})
Awareness: {distribuição por nível}
Status atualizado: 🧠 Estratégia p/ Revisar
```

Em modo webhook, gravar log mínimo no console do RemoteTrigger.

## Regras de qualidade (não negociáveis)

1. **Zero travessões (—)** em qualquer texto gerado. Usar vírgula, ponto ou `|` no lugar.
2. **Toda afirmação técnica** que entrar nas Copies precisa ser passível de citação (CFM, sociedade médica da especialidade, paper). Na estratégia em si as Copies são resumos, mas elas serão expandidas pelo Trigger 2 com referências obrigatórias.
3. **Conformidade Res. CFM 2.336/2023** — não negociável. Listar nas validações operacionais a aderência.
4. **Voz do cliente** — tom, bordões, analogias, hooks favoritos vêm do Perfil V1. Não inventar voz nova.
5. **CTAs do banco do cliente** — não criar CTAs novos. Se um post pede CTA fora do banco, sinalizar nas validações.
6. **Distribuição equilibrada** — sem 3 conteúdos do mesmo pilar em uma única semana, sem 2 conteúdos com hooks idênticos no ciclo, sem CTAs repetidos consecutivamente.
7. **Hooks fortes nos primeiros 3s** — pergunta direta, mito-vs-verdade, dado provocativo, contradição. Não começar Reel com saudação genérica ("Oi gente, hoje vou falar sobre...").

## Quando parar e perguntar (comentar no card e abortar)

- Cliente sem Perfil V1 ou campos críticos vazios
- Período inválido (start > end, ou < 7 dias)
- Postagens por semana = 0 ou ausente
- Modo `refazer` mas callout de feedback vazio
- Conflito entre Orientações do mês e Proibições do cliente (ex: SM pediu "post antes/depois" mas Proibição "Antes/Depois" tá ativa)
- Status do card incompatível com a invocação (ver "Detecção de modo")

## Pilotos / testes

Card de referência: **Roteiros Dra Larissa** (DEM-10) — `https://www.notion.so/Roteiros-Dra-Larissa-34e627656baf80269091ead501ccc852`. Subpágina de estratégia gerada manualmente serve como gold standard de comparação.

Primeiro teste recomendado: rodar em modo `gerar` num card novo da Dra. Larissa pra Junho/Julho 2026 e comparar visualmente o output com a subpágina existente de Maio/Junho.
