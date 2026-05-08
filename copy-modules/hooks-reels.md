# Hooks de Reel — John Carlton + adaptação médica do criador-de-hooks-para-reels

> Módulo de execução. Lido pela `/AM_roteiros` toda vez que gerar um Reel.

## Princípio

Os primeiros 3 segundos decidem retenção. **Carlton:** "Stop the scroll. Start the engine." A diferença entre um Reel que faz 5 mil views e um que faz 500 mil é o hook.

No marketing médico, o hook tem um filtro adicional: **não pode comprometer autoridade**. Hook clickbait queima credibilidade. Hook genérico ("oi gente, hoje vou falar sobre...") perde retenção.

A regra é: **hook que para o scroll sem ferir a autoridade do médico.**

## 4 categorias de hook (adaptadas do criador-de-hooks-para-reels e da escola Carlton)

### Categoria A — Curiosidade (mistério clínico, número intrigante)

**Princípio:** abre uma pergunta que o leitor PRECISA fechar, com aporte de dado ou mistério clínico real.

**Templates:**
- "Tem uma frase que eu escuto toda semana no consultório. E ela diz muito mais do que parece."
- "Hoje é Dia [X]. E eu preciso te contar o que [tema] faz com [parte do corpo]."
- "Existe uma confusão que vejo quase toda semana sobre [tema]. Vou desmontar agora."
- "Se você [sintoma], o seu corpo pode estar te dizendo algo que poucos médicos param pra ouvir."

**Visual sugerido:** plano fechado no médico (não está sorrindo), expressão de "deixa eu te contar uma coisa".

**Por que funciona:** deixa um loop aberto que o cérebro do leitor exige fechar.

**Use quando:** Reel educativo que vai desenvolver tese, Reel de início de série, Reel sobre tema desconhecido pelo público.

### Categoria B — Identificação (sintoma reconhecível, frase do paciente)

**Princípio:** o leitor pensa "isso é sobre mim" nos primeiros 2 segundos. Carlton chama isso de "the prospect recognizes himself in the headline".

**Templates:**
- "Seu olho está mais fechado que o outro?"
- "Se você cruza as pernas o dia todo, deixa eu te mostrar o que acontece."
- "Tem algum desses sintomas no fim do dia? Peso, inchaço, cansaço visual?"
- "[Frase real que o paciente diz no consultório, em forma de pergunta retórica]"

**Visual sugerido:** olhar direto na câmera, gesto de apontar (sem ser acusatório), expressão acolhedora.

**Por que funciona:** ativa o sistema de reconhecimento ("eu tenho isso") antes de qualquer racionalização.

**Use quando:** Reel direcionado a Problem Aware (público sente, mas não nomeou). Maioria dos Reels de Autoridade.

### Categoria C — Mito-vs-verdade clínico (substitui "choque/controvérsia")

**Princípio:** Carlton inventou o "anti-pitch hook" — o que parece anti-vendedor desperta curiosidade. No médico, isso vira **derrubar mito popular com base técnica**.

**Templates:**
- "[Mito popular] não é verdade. Vou te explicar com fonte."
- "Castanha-da-índia, hibisco, mirtilo. Esses chás viralizam todo ano como 'cura pra X'. Vou ser justo com a ciência."
- "[Crença comum] é o que mais atrasa diagnóstico. Tira 1 minuto e leia."
- "Tem uma palavra que eu evito quando falo de [procedimento]. E eu evito de propósito."

**Visual sugerido:** split screen (mito de um lado, verdade do outro) ou expressão de "deixa eu te corrigir".

**Por que funciona:** pattern interrupt cognitivo. O cérebro espera o mito ser confirmado, e ouve o oposto.

**Use quando:** Reel react, conteúdo educativo de Solution Aware, posicionamento contra desinformação viral.

**CRITICAL:** o mito desmontado tem que ser falsamente popular E desmontado COM fonte. Se for desmontagem opinativa sem fonte, vira ataque pessoal a colegas. **Banido.**

### Categoria D — Pergunta direta (frase-do-consultório)

**Princípio:** Carlton repetia "the more conversational the headline, the better". A pergunta que o paciente faria pessoalmente vira hook.

**Templates:**
- "Como funciona a primeira avaliação aqui no consultório?"
- "Por que escolhi a [especialidade]?"
- "Quanto tempo dura uma cirurgia [X]?"
- "[Pergunta que o médico ouve toda semana]"

**Visual sugerido:** bastidor (consultório, equipamento, médico em movimento natural), tom conversacional.

**Por que funciona:** o paciente lê e sente que tá numa conversa, não numa propaganda.

**Use quando:** Reel de Conversão direta, conteúdo de demonstração ("como é aqui"), Reel pessoal/manifesto.

## Estrutura de output que a `/AM_roteiros` deve gerar

Pra cada Reel, **3 opções de hook** (uma de cada categoria distinta), pra que o médico escolha. Apresentar como:

```
**Hook (3 opções):**
A. [Categoria]: "[texto]"
B. [Categoria]: "[texto]"
C. [Categoria]: "[texto]"
**Recomendado:** opção [letra] — [justificativa em 1 linha]
```

A recomendação considera o nível de consciência do conteúdo (ver `awareness-calibration.md`).

## Critérios de qualidade (Carlton's checklist)

Pra cada hook gerado, validar:

- [ ] **Cabe em 3 segundos** falados em ritmo normal (8-15 palavras)
- [ ] **Não começa com saudação genérica** ("oi gente", "bom dia", "olá")
- [ ] **Tem zero travessão**
- [ ] **Não promete resultado**
- [ ] **Não ataca colega/concorrente**
- [ ] **Cabe na voz do médico** (compatível com tom + bordões + analogias do Perfil V1)
- [ ] **Conecta com a Big Idea do mês** (ver `big-idea-mensal.md`)
- [ ] **Tem progressão** (a primeira frase puxa pra próxima — ver `slippery-slide.md`)

## Anti-padrões (NÃO fazer)

- "Oi pessoal, tudo bem? Hoje eu vim falar sobre..." (perde 80% do scroll em 3s)
- "Você sabia que..." (genérico, fraco)
- "ATENÇÃO! Não cometa esse erro!" (clickbait, queima autoridade)
- "Esse vídeo VAI MUDAR a sua vida" (promessa exagerada)
- "Médico revela segredo que clínicas não querem que você saiba" (conspiração, banido pelo CFM)
- Hook puro emoção sem ancoragem clínica ("você sente que falhou?")
- Hook que termina com promessa de resultado ("você vai voltar a ser quem era")

## Banco de hooks por especialidade (extraível de cards reais)

A `/AM_roteiros` mantém um banco crescente de hooks que **funcionaram** em ciclos anteriores, agrupados por especialidade. Hooks da Dra. Larissa (oftalmoplástica) servem de base, e novos hooks aprovados em revisões viram acervo da especialidade.

Esse banco fica anexado ao Perfil V1 do cliente no campo "Hooks favoritos".
