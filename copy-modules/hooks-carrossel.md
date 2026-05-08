# Hooks de Carrossel — Gary Bencivenga + adaptação médica do criador-de-hooks-para-carrossel

> Módulo de execução. Lido pela `/AM_roteiros` toda vez que gerar um Carrossel.

## Princípio

Capa decide se a pessoa desliza. **Bencivenga:** "Curiosity is the fuel; specificity is the fire." Capa específica + curiosa = swipe garantido. Capa genérica = scroll.

Carrossel médico tem perfil único: alto **salvamento** e **compartilhamento**. Não é pra "viralizar com humor" (proibição padrão da agência). É pra **virar referência consultável** (paciente salva pra revisitar antes de marcar consulta).

A capa é o título de um livro que o leitor vai abrir. Não é manchete tabloide.

## Anatomia da capa de carrossel médico

```
┌──────────────────────┐
│                      │
│   [TEXTO PRINCIPAL]  │ ← 6-12 palavras, frase de impacto
│                      │
│   [SUBTEXTO]         │ ← 4-8 palavras, contexto/promessa de método
│                      │
│   [Logo + identidade]│ ← assinatura visual da clínica
│                      │
└──────────────────────┘
```

## 4 categorias de capa (adaptadas do criador-de-hooks-para-carrossel e da escola Bencivenga)

### Categoria A — Educativa (didática, autoridade)

**Princípio:** anuncia que o leitor vai sair sabendo algo concreto. Bencivenga chama isso de "the offer is information itself".

**Templates:**
- "[N] sinais de que sua [parte do corpo] pede atenção"
- "[Procedimento]: o que esperar nos primeiros [X] dias"
- "Quando 1 procedimento basta. E quando combinar faz sentido."
- "Como funciona [técnica/exame] na prática"

**Subtexto sugerido:** "{contexto}. Em N slides."

**Tom:** professoral mas acessível. Sem jargão sem definir.

**Use quando:** conteúdo de Autoridade pura, público Problem/Solution Aware, tema técnico que merece desenvolvimento.

### Categoria B — Lista numerada (alta densidade de informação)

**Princípio:** cérebro adora lista. Número específico promete escopo definido. **Bencivenga:** "Specific numbers beat round numbers." (5 sinais > muitos sinais)

**Templates:**
- "5 sinais de que sua circulação pede atenção"
- "3 erros de quem trata [X] sem exame"
- "7 hábitos que maltratam suas pernas silenciosamente"
- "4 frases que eu mais escuto no consultório"

**Subtexto sugerido:** geralmente dispensável, mas pode reforçar relevância: "Sinais que apareceram em 8 de cada 10 consultas."

**Tom:** direto, listável, escaneável.

**Use quando:** sintomas, erros, hábitos, mitos, perguntas frequentes.

**Critério Bencivenga:** o número tem que ser **específico e justificável**. 5 sinais = exatamente 5 slides com 5 sinais (não 4, não 6).

### Categoria C — Pergunta clínica (consulta na capa)

**Princípio:** capa-pergunta puxa o leitor pra dentro do carrossel pra encontrar a resposta. Tipo Bencivenga: "Pose the question they're already asking themselves."

**Templates:**
- "Seu olho está mais fechado que o outro?"
- "Variz é só estética?"
- "Blefaroplastia dói?"
- "Quando procurar um cirurgião vascular?"

**Subtexto sugerido:** "A resposta depende de 3 coisas." | "Vou te mostrar o que avaliar."

**Tom:** conversacional, escuta ativa, como se a paciente estivesse no consultório.

**Use quando:** dúvida frequente, mito a desmontar com nuance, conteúdo Problem/Solution Aware.

### Categoria D — Mito-vs-verdade clínico (capa-correção)

**Princípio:** capa que enuncia mito + sinaliza correção. Bencivenga: "Promise the prospect you'll save them from a costly mistake."

**Templates:**
- "[Mito popular]: a verdade que ninguém explica"
- "Por que [crença comum] está te custando caro"
- "[Frase típica do paciente]: o problema com essa lógica"
- "[Procedimento] não é o que você pensa que é"

**Subtexto sugerido:** "{Tipo de evidência}: vamos ver a ciência." | "O que muda quando você entende o mecanismo."

**Tom:** firme mas não condescendente. Corrige sem ridicularizar.

**Use quando:** desinformação viral relevante, mito clínico atrapalhando decisão de paciente.

## Estrutura ideal de carrossel médico (9 slides)

A `/AM_roteiros` deve gerar carrosséis com 9 slides como padrão (5-10 aceitável conforme tema):

```
Slide 1 — Capa (escolhida das 4 categorias acima)
Slide 2 — Contexto/setup do tema (1-2 frases)
Slide 3-7 — Desenvolvimento (1 conceito por slide, máx 30-50 palavras cada)
Slide 8 — Síntese ou nuance importante (o "porém", o "mas atenção")
Slide 9 — CTA (chamada pra avaliação ou salvar)
```

**Princípio Bencivenga aplicado:** cada slide tem UM conceito. Não amontoa. O carrossel não é PowerPoint compactado, é livro ilustrado.

## Estrutura de output que a `/AM_roteiros` deve gerar

Pra cada Carrossel, **3 opções de capa** (de categorias distintas) + estrutura completa dos 9 slides:

```
**Capa (3 opções):**
A. [Categoria Educativa]: "{texto}" | sub: "{subtexto}"
B. [Categoria Lista]: "{texto}" | sub: "{subtexto}"
C. [Categoria Pergunta]: "{texto}" | sub: "{subtexto}"
**Recomendada:** opção [letra] — [justificativa]

**Slides:**
**Slide 01 (Capa):** {texto da opção recomendada}
**Slide 02:** {desenvolvimento}
...
**Slide 09 (CTA):** {chamada}
```

## Critérios de qualidade (Bencivenga's checklist)

Para cada capa gerada, validar:

- [ ] **Texto principal**: máximo 12 palavras
- [ ] **Subtexto**: máximo 8 palavras (ou ausente)
- [ ] **Específica**: tem número, nome técnico, ou ângulo concreto
- [ ] **Curiosa**: deixa pergunta aberta que o leitor quer fechar
- [ ] **Honesta**: não promete o que o carrossel não entrega
- [ ] **Sem clickbait**: sem "VOCÊ NÃO VAI ACREDITAR", "MÉDICO REVELA", etc
- [ ] **Sem promessa de resultado**
- [ ] **Voz do cliente** (Tom + Bordões do Perfil V1)
- [ ] **Conecta com a Big Idea do mês**

## Anti-padrões (NÃO fazer)

- "Tudo o que você precisa saber sobre X" (genérico, sem ângulo)
- "X coisas incríveis que mudarão sua vida" (vazio + clickbait)
- "MÉDICO REVELA SEGREDO que clínicas escondem" (conspiração + banido CFM)
- Capa sem conteúdo (só estética, sem promessa de informação)
- Capa que promete o que o carrossel não entrega (bait-and-switch)
- Capa com mais de 12 palavras (vira ilegível mobile)

## Banco de capas validadas por especialidade

Cada carrossel aprovado em revisão alimenta o banco de capas da especialidade do cliente, anexado ao Perfil V1 (campo "Hooks favoritos" também guarda capas de carrossel que funcionaram).

A `/AM_roteiros` consulta esse banco antes de gerar nova capa, pra:
1. Evitar repetição de tema/ângulo recente
2. Reusar variações de capas que comprovadamente engajaram
3. Identificar formatos que NÃO funcionam pra essa especialidade

## Render visual

A capa renderizada vai pra produção visual (designer da agência). A `/AM_roteiros` entrega:

```
**Capa (cliente):** {texto principal} {| subtexto se houver}
**Capa (videomaker):** Não precisa gravar.  ← (carrossel não tem captura de vídeo)
```

A direção visual (paleta, tipografia, mood) vem do Perfil V1 do cliente. Não da skill.
