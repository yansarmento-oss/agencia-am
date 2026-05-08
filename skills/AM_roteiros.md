# am-roteiros

Geração (ou re-geração com feedback) da subpágina **Roteiros** dentro de um card de Demanda no Sistema A.M. de Roteiros. Disparada por mudança de status no DB Demandas. Produz roteiros completos pra cada conteúdo do ciclo (Reels com roteiro 240-280 palavras + Carrosséis 9 slides), comandados pela Copy Squad: Hopkins + Schwartz na execução, com módulos especializados de Carlton (hooks-reels), Bencivenga (hooks-carrossel), Sugarman (slippery-slide), Kennedy soft (CTAs), Hopkins (proof). Segue template canônico (Dra. Larissa DEM-10).

---

## Contexto (ler antes de executar)

- DB Demandas: data source `collection://d65eac39-53cd-4a76-87d3-87988ff0b1cc`
- DB Clientes: data source `collection://9a66bcde-df8a-4286-bd25-e7e592660e11`
- Parent page: "A.M. - Sistema de Roteiros" (`341627656baf8085a6f8fa2c18a1aad8`)

**Memórias obrigatórias:**
- `reference_am_estrategia_roteiros_template.md` — template canônico de roteiros (Larissa DEM-10)
- `feedback_am_kanban_visual_convention.md` — semântica dos status
- `feedback_writing_style_yan.md` — zero travessões + referências citadas
- `feedback_notion_strategy_formatting.md` — sintaxe Notion (toggle verde, callout, emojis)
- `feedback_roteiro_duration.md` — Reels 60-90s, 240-280 palavras
- `project_medical_marketing_agency.md` — autoridade médica acima de viralização

**Copy modules obrigatórios** em `/Users/Usuario/agencia-am/copy-modules/`:
- `big-idea-mensal.md` — Big Idea do ciclo (definida pela `/AM_estrategia`, lida aqui)
- `awareness-calibration.md` — Schwartz, calibração de tom por nível de consciência
- `proof-pattern.md` — Hopkins, hierarquia de fontes científicas (CFM > sociedade BR > internacional > paper)
- `hooks-reels.md` — Carlton + criador-reels adaptado, 4 categorias de hook pra Reel
- `hooks-carrossel.md` — Bencivenga + criador-carrossel adaptado, 4 categorias de capa
- `slippery-slide.md` — Sugarman, cadência do Reel 240-280 palavras
- `ctas-medicos.md` — Kennedy soft, banco de CTAs por intenção

## Squad command

Esta skill é executada sob comando da Copy Squad (filtrada para marketing médico):

| Especialista | Função | Onde atua |
|---|---|---|
| **Eugene Schwartz** | Cérebro intelectual | Calibra cada conteúdo conforme awareness-calibration |
| **Claude Hopkins** | Rigor científico | Toda afirmação técnica passa pelo proof-pattern |
| **John Carlton** | Hooks de Reel | Geração de 3 opções de hook por Reel (hooks-reels) |
| **Joe Sugarman** | Slippery slide | Cadência de 240-280 palavras (slippery-slide) |
| **Gary Bencivenga** | Capas + bullets | Geração de 3 opções de capa por Carrossel (hooks-carrossel) |
| **Dan Kennedy (soft)** | CTAs | Banco de CTAs por nível de consciência (ctas-medicos) |

A skill **não delega** pra esses especialistas em runtime — incorpora os métodos deles no próprio prompt e nos módulos de referência.

## Input esperado

A skill recebe (via RemoteTrigger payload do webhook Notion, ou invocação manual):

- **page_id** ou **page_url** — card de Demanda no DB Demandas
- (opcional) **modo** — `gerar` ou `refazer`. Se omitido, detecta pelo status atual.

## Detecção de modo pelo status

Lê `Status` do card:

- `🤖 Gerando Roteiros` → modo **gerar** (primeira versão)
- `🤖 Refazendo Roteiros` → modo **refazer** (loop com feedback)
- Qualquer outro status → **abort**, comentar no card "Skill `/AM_roteiros` disparada com status incorreto: {status}. Esperado: 🤖 Gerando ou 🤖 Refazendo Roteiros." e parar.

## Pré-condição obrigatória

A subpágina de Estratégia já deve existir (filha do card, título começando com `🧠 Estratégia`). Se não existir: comentar no card "Subpágina de Estratégia não encontrada. Rode `/AM_estrategia` antes." + abort.

## Fluxo de execução

### Fase 1 — Coleta de dados

1. **Fetch do card de Demanda.** Extrair:
   - `Demanda` (título), `Demanda ID`
   - `Cliente` (relation → URL da página do cliente)
   - `Período`, `Postagens por semana`, `Orientações do mês`, `Referências/Links`, `Aprovador`, `Status`

2. **Fetch da subpágina de Estratégia.** Extrair (toda a estrutura é critical input pros roteiros):
   - **Big Idea do ciclo** (bloco no topo): tese, mecanismo único, promessa de método, CTA do mês
   - **Datas-chave** do período
   - **Pilares ativados** + **Territórios discursivos**
   - **Calendário completo** com cada conteúdo: número, formato, camada, awareness, relação com Big Idea, eixo, hook proposto, copy/estrutura sugerida, CTA proposto
   - **Validações operacionais** (mix final, distribuição)

3. **Fetch do card de Cliente.** Extrair TODOS os campos relevantes:
   - **Identidade:** Nome, Especialidade, Subnicho, Localização, RQE (todos), Assinatura padrão
   - **Voz:** Tom de voz, Arquétipo, Bordões, Analogias, Hooks favoritos, CTAs padrão
   - **Posicionamento:** Público-alvo, Nível de consciência, Mix autoridade/conversão, Produto-âncora, Tagline
   - **Governança:** Proibições (multi-select), Temas proibidos
   - **Visual (referência pra capa videomaker):** Cores primárias, Cores secundárias, Tipografia, Mood visual
   - **Perfil V1 completo** (corpo da página) — pilares de conteúdo, tese autoral, frases recorrentes, voz real (se há transcrições frame.io)

4. **Validar inputs:**
   - Subpágina de estratégia tem Big Idea preenchida (4 elementos: tese, mecanismo, promessa, CTA do mês)
   - Calendário tem N conteúdos coerentes com Postagens/semana × semanas
   - Cliente tem todos os campos críticos

   Se faltar qualquer um: comentar no card listando o problema + abort sem alterar status.

### Fase 2 — Modo gerar (escrita do zero)

Para cada conteúdo do calendário (na ordem em que aparecem na estratégia):

#### 2.1 — Determinar tipo (Reel ou Carrossel)

Lido do callout da estratégia. Cada formato segue rota diferente.

#### 2.2 — Geração de Reel

Estrutura completa de cada Reel (callout dentro do toggle da semana):

```markdown
**Conteúdo NN · Reel · {Camada} · {Eixo}**
**Hook:** {frase de 8-15 palavras, escolhida pela skill entre 3 opções}
**Capa (cliente):** {texto curto pra capa visual — 3-6 palavras}
**Capa (videomaker):** {descrição da cena — plano, ambiente, paleta do cliente, texto sobreposto, tipografia}
**Notas de gravação:** (opcional) {tom, ritmo, pausas, planos de corte}
**Roteiro:** {texto de 240-280 palavras, 1ª pessoa, voz do médico, slippery slide}
**Legenda:**
{3-5 parágrafos curtos}

{CTA final, do banco do cliente}

*{Nome completo} \| CRM {n} \| RQE {n} ⚠️ Esse conteúdo é informativo e não substitui uma consulta com um médico especialista.*

**Referências:**
- {fonte 1: regulação, sociedade médica, paper}
- {fonte 2}
```

**Ordem de geração de cada Reel:**

1. **Hook (3 opções, escolha + justificativa)** — segue `hooks-reels.md`. Categorias: A-Curiosidade, B-Identificação, C-Mito-vs-verdade clínico, D-Pergunta direta. A escolha bate com nível de consciência do conteúdo (ver `awareness-calibration.md` tabela rápida). Output:
   ```
   **Hook (3 opções):**
   A. [Curiosidade]: "..."
   B. [Identificação]: "..."
   C. [Mito-vs-verdade]: "..."
   **Recomendado:** B — [justificativa em 1 linha, baseada em awareness]
   **Hook usado:** "{texto da opção recomendada}"
   ```
   No corpo final do callout, vai apenas a opção recomendada como `**Hook:**`. As 3 opções ficam num sub-bloco de revisão (toggle "💡 Variantes de hook" colapsado, pra revisor ver alternativas se quiser trocar).

2. **Capa cliente** — texto curto da capa visual (geralmente 3-6 palavras), derivado do hook. Mesma estrutura de fonte que aparece no perfil do médico. Ex: hook "Não existe cirurgia simples. Existe cirurgia bem feita." → capa cliente "Cirurgia simples?" (pergunta retórica, deixa curiosidade pra ver Reel).

3. **Capa videomaker** — descrição visual da cena pra produção. Inclui: plano (fechado/médio/aberto), ambiente (consultório/home/clínica), paleta (puxada das Cores primárias/secundárias do Cliente), texto sobreposto + tipografia (puxada da Tipografia do Cliente). Ex: "Plano fechado da médica em ambiente clínico com paleta bege e dourada predominante. Texto sobreposto Cirurgia simples? em serif itálico dourado."

4. **Notas de gravação (opcional)** — quando o tom precisa de orientação específica (Reels mais íntimos, manifestos, bastidor). Pode incluir: ritmo, pausas, expressão, sequência de cortes. Ex: "Tom mais pessoal e suave do que os reels educativos. Pausas mais longas antes do agradecimento final."

5. **Roteiro completo (240-280 palavras)** — segue `slippery-slide.md`:
   - 1ª pessoa (voz do médico)
   - Tom + bordões + analogias do Cliente
   - Estrutura: hook (já escolhido) → setup ultracurto → desenvolvimento (3-5 parágrafos curtos) → peak → CTA
   - Big Idea aparece 3x (latente no hook, direta no peak, reforço no fechamento)
   - Cada parágrafo = 1 ideia + gancho pro próximo
   - Pelo menos 1 número/dado específico ancorando alguma frase
   - Pelo menos 1 quebra de expectativa
   - Zero travessão
   - Frases curtas alternando com longas (axe-cutting rhythm)

6. **Legenda** (3-5 parágrafos):
   - Primeiro parágrafo: contextualiza o tema sem repetir o Reel
   - Parágrafos do meio: complementam ou aprofundam
   - Parágrafo final: insight ou peça-chave da Big Idea
   - CTA do cliente (do banco) ao final do texto
   - **Assinatura padrão obrigatória** (CRM + RQE + disclaimer, ver `proof-pattern.md`)

7. **Referências** — todas as fontes citadas no Reel ou que sustentam afirmações técnicas. Hierarquia: CFM > sociedade BR > internacional > paper. Formato Vancouver pros papers. Ver `proof-pattern.md`.

#### 2.3 — Geração de Carrossel

Estrutura completa de cada Carrossel (callout dentro do toggle da semana):

```markdown
**Conteúdo NN · Carrossel · {Camada} · {Eixo}**
**Hook:** {frase da capa}
**Capa (cliente):** {texto principal da capa, max 12 palavras + subtexto opcional max 8 palavras}
**Capa (videomaker):** Não precisa gravar.
**Slides:**
**Slide 01 (Capa):** {texto da capa, geralmente repete ou refraseia o hook}
**Slide 02:** {desenvolvimento — 30-50 palavras}
**Slide 03:** {desenvolvimento}
... [até Slide 09]
**Slide 09 (CTA):** {chamada final}
**Legenda:**
{3-5 parágrafos curtos}

{CTA do banco do cliente}

*{Assinatura padrão CRM/RQE/disclaimer}*

**Referências:**
- {fontes}
```

**Ordem de geração de cada Carrossel:**

1. **Hook capa (3 opções, escolha + justificativa)** — segue `hooks-carrossel.md`. Categorias: A-Educativa, B-Lista numerada, C-Pergunta clínica, D-Mito-vs-verdade. Escolha bate com awareness. Output similar ao Reel (3 opções no toggle "💡 Variantes de capa").

2. **Capa cliente** — texto principal (até 12 palavras) + subtexto (até 8 palavras, opcional).

3. **Capa videomaker** — sempre `Não precisa gravar.` (carrosséis são produzidos no design, não em vídeo).

4. **Slides 1-9** — segue estrutura de `hooks-carrossel.md`:
   - Slide 1 = Capa (texto da capa)
   - Slides 2-8 = Desenvolvimento (1 conceito por slide, 30-50 palavras cada)
   - Slide 9 = CTA
   - Cada slide é escaneável, 1 ideia, sem amontoar
   - Bencivenga: "specific numbers beat round numbers" (5 sinais ≠ "vários")

5. **Legenda** + **Assinatura** + **Referências** — mesma estrutura do Reel.

#### 2.4 — Validações por conteúdo (rodar pra cada um antes de escrever)

Pra **cada Reel**, validar checklist `slippery-slide.md`:
- [ ] 240-280 palavras
- [ ] Hook não começa com saudação genérica
- [ ] Cada parágrafo termina com gancho
- [ ] Big Idea aparece 3x
- [ ] Pelo menos 1 número específico
- [ ] Zero travessão
- [ ] CTA coerente com awareness

Pra **cada Carrossel**, validar checklist `hooks-carrossel.md`:
- [ ] Capa max 12 palavras + sub max 8
- [ ] 1 ideia por slide
- [ ] Slide 9 é CTA claro
- [ ] Sem clickbait

Pra **todos**, validar `proof-pattern.md`:
- [ ] Toda afirmação técnica tem fonte
- [ ] Hierarquia respeitada (CFM > sociedade BR > internacional > paper)
- [ ] Formato Vancouver em papers
- [ ] Disclaimer assinado em legenda
- [ ] CRM + RQE visíveis

E `ctas-medicos.md`:
- [ ] CTA único (não 3-5 ações)
- [ ] Canal específico nomeado
- [ ] Zero urgência fake
- [ ] Zero promessa de resultado
- [ ] CTA do banco do cliente

### Fase 3 — Escrita no Notion (modo gerar)

1. Criar subpágina filha do card:
   - Título: `✍️ Roteiros {MesIniAbrev}/{MesFimAbrev} {Ano} — {NomeCliente}` (ex: `✍️ Roteiros Maio/Jun 2026 — Dra. Larissa`)

1.1. **Atualizar a seção `## Roteiros` do card pai.** Após criar a subpágina, usar `notion-update-page` com `update_content` pra trocar o placeholder atual da seção "## Roteiros" (geralmente texto em itálico "*Subpágina de roteiros será criada automaticamente...*") pelo link da subpágina nova:

```
## Roteiros
<page url="{URL_DA_SUBPAGINA_CRIADA}">✍️ Roteiros {Mes}/{Mes+1} {Ano} — {NomeCliente}</page>
```

**CRITICAL:** preservar os callouts "## Feedback do aprovador" e "## Feedback dos roteiros" intactos — eles podem ter conteúdo do revisor. Usar search-and-replace cirúrgico, não `replace_content` global.

2. Conteúdo segue **fielmente** o template em `reference_am_estrategia_roteiros_template.md` seção "Subpágina de Roteiros — template":
   - Quote header: `> **Ciclo:** {datas} · {N} conteúdos ({X} Reels de 60-90s + {Y} Carrosséis) · Duração-alvo Reels: **240-280 palavras**`
   - Bloco "Como revisar este documento" (instruções ao revisor)
   - Separador `---`
   - Por semana: heading 3 toggle verde com mesmo título/tema da estratégia
   - Por conteúdo: callout (🎬 Reel / 🎠 Carrossel) com toda a estrutura completa (hook, capas, roteiro/slides, legenda, assinatura, referências)
   - Para cada Reel/Carrossel, **dentro do callout** incluir um sub-toggle colapsado `💡 Variantes de hook` ou `💡 Variantes de capa` com as 3 opções geradas + justificativa da escolha (pro revisor poder trocar se quiser).

3. **Atenção técnica de renderização Notion:**
   - Usar `{toggle="true" color="green_bg"}` no heading 3 das semanas
   - Indentar conteúdo do toggle com 1 tab
   - Callout: `<callout icon="🎬">` ou `<callout icon="🎠">`, conteúdo indentado 1 tab dentro
   - Sub-toggle dentro do callout: heading h4 toggle padrão
   - Caracteres reais (newlines, emojis) — nunca `\n` ou `\uXXXX`
   - Pipe escapado como `\|` em assinaturas
   - Quebras de linha entre parágrafos da legenda usar linha em branco (não `<br>`)

### Fase 4 — Modo refazer (loop com feedback)

1. Buscar a subpágina de Roteiros já existente (filha do card, título começando com `✍️ Roteiros`).
2. Ler o callout "Feedback dos roteiros" do card pai. Se vazio: comentar "Status `🤖 Refazendo Roteiros` mas callout de feedback vazio. Sem orientações pra refazer." + abort.
3. Ler conteúdo atual da subpágina (V1 ou versão mais recente).
4. **Identificar escopo do feedback:**
   - **Feedback global** (afeta todos os conteúdos): ex "Tirar todos os travessões", "Adicionar referências em afirmações técnicas"
   - **Feedback por conteúdo** (afeta itens específicos): ex "Conteúdo 4: expandir analogia do iceberg"
   - **Feedback misto** (combina global + específico)
5. **Aplicar correções:**
   - Feedback global: rodar correção em todos os 15 conteúdos
   - Feedback específico: refazer só o conteúdo apontado, mantendo os outros
6. **Preservar histórico:** dentro da subpágina, criar (ou atualizar) toggle no FINAL `📚 Versões anteriores` (gray_bg). Mover conteúdo da V atual pra dentro, prefixado por `### V{N} · gerada em {data ISO}` (incrementar N).
7. Reescrever o corpo principal com a nova versão.
8. Adicionar nota no início (após quote header): `> **Versão {N+1}** · refeita em {data ISO} aplicando feedback do aprovador. Mudanças aplicadas: [resumo curto]. Histórico no toggle "📚 Versões anteriores" no final.`
9. **Adicionar checklist de revisão** (estilo card Reinaldo DEM-9) ao final do documento, mostrando o que foi corrigido:
   ```
   ### ✅ Checklist de revisão V{N+1} (respondendo ao feedback)
   - [x] {item do feedback} → {como foi resolvido}
   - [x] ...
   ```

### Fase 5 — Atualização de status e card pai

1. Atualizar `Status` do card pai pra `✍️ Roteiros p/ Revisar`.
2. Comentar no card mencionando o `Aprovador`: `Roteiros {gerados/refeitos V{N+1}} e prontos pra revisão. {N} conteúdos. Big Idea aplicada: "{tese}".`
3. **NÃO** apagar o callout "Feedback dos roteiros" — ele continua disponível pra próxima rodada.

### Fase 6 — Output final (log)

Em modo invocação manual:

```
✅ Roteiros {gerados/refeitos V{N+1}} — {NomeCliente} {MesIni}/{MesFim} {Ano}
Big Idea aplicada: "{tese}"
Card: {URL}
Subpágina: {URL}
Conteúdos: {N} ({X Reels com média de palavras} + {Y Carrosséis com média de slides})
Referências citadas: {total} fontes ({X CFM, Y sociedades médicas, Z papers})
Status atualizado: ✍️ Roteiros p/ Revisar
```

Em modo webhook, log mínimo no console RemoteTrigger.

## Regras de qualidade (não negociáveis)

1. **Zero travessões (—)** em qualquer texto gerado. Usar vírgula, ponto ou `\|` no lugar.
2. **Toda afirmação técnica** com fonte citada nas Referências. Sem exceções. Ver `proof-pattern.md` e hierarquia.
3. **Conformidade Res. CFM 2.336/2023** — não negociável. Sem promessa de resultado, sem urgência fake, sem antes/depois sem consentimento + RQE, sem ataque a colega/concorrente.
4. **Voz do cliente** (Tom + Bordões + Analogias + Hooks favoritos do Perfil V1). Nunca inventar voz.
5. **CTAs do banco do cliente** (campo CTAs padrão do DB Clientes). Não criar CTA novo sem motivo.
6. **Big Idea do mês aparece 3x em cada Reel** (hook latente + peak + fechamento) e 1x na capa de cada Carrossel.
7. **Reels: 240-280 palavras**. Sem exceção (60-90s falados).
8. **Carrosséis: 9 slides padrão** (5-10 aceitável conforme tema, mas 9 é default).
9. **Hooks fortes nos primeiros 3s**. Categorias permitidas: Curiosidade, Identificação, Mito-vs-verdade, Pergunta direta. Banidos: saudação genérica, clickbait, conspiração, promessa.
10. **Awareness calibrado por conteúdo** — hook + tom + CTA batem com o nível de consciência alvo (definido na estratégia).
11. **Disclaimer assinado em toda legenda** (CRM + RQE + disclaimer padrão).
12. **3 opções de hook/capa por conteúdo** salvas em sub-toggle pra revisor poder trocar.

## Quando parar e perguntar (comentar no card e abortar)

- Subpágina de Estratégia não existe (rodar `/AM_estrategia` antes)
- Big Idea ausente ou incompleta na estratégia (rodar `/AM_estrategia` em modo refazer)
- Cliente sem Perfil V1 ou campos críticos vazios
- Calendário da estratégia tem inconsistência (N conteúdos ≠ semanas × postagens/semana)
- Modo `refazer` mas callout de feedback vazio
- Conflito entre feedback do aprovador e Proibições do cliente
- Status do card incompatível com a invocação

## Pilotos / testes

Card de referência: **Roteiros Dra Larissa** (DEM-10) — subpágina `✍️ Roteiros Maio/Jun 2026 — Dra. Larissa` é gold standard.

Card secundário: **Reinaldo - Maio** (DEM-9) — versão mais antiga do template (template V1, dentro da página principal e não em subpágina). Útil pra ver: feedback do aprovador real (zero travessões + referências), checklist V2 de revisão, formato de Reel completo com referências bibliográficas, peças sazonais (Dia do Tabaco, São João, Dia das Mães).

Primeiro teste recomendado: rodar em modo `gerar` num card cuja `/AM_estrategia` já tenha rodado (com Big Idea preenchida). Comparar visualmente com o template Larissa.

## Métricas de sucesso (review do output)

A Copy Chief (Cyrus) avalia o output dos roteiros pelos 8 critérios universais:

1. Hook para o scroll? (Schwartz/Carlton test)
2. Lead compelling nos primeiros 3 segundos? (Halbert test adaptado)
3. Específico e concreto? (Ogilvy/Hopkins test)
4. Cada frase puxa a próxima? (Sugarman test)
5. CTA claro e irresistível? (Kennedy test soft)
6. Bullets/slides carregados de curiosidade? (Bencivenga test pra carrosséis)
7. Fechamento com peak antes do CTA? (Sugarman test)
8. Você marcaria a consulta se fosse o paciente? (Universal test)

Se 2+ critérios falharem em mais de 30% dos conteúdos: skill devolve com diagnóstico, não escreve no Notion.
