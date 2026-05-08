---
name: AM Sistema de Roteiros — Templates de Estratégia e Roteiros (gold standard)
description: Template canônico (validado em campo, ex: Roteiros Dra. Larissa DEM-10 Maio/Jun 2026) das duas subpáginas que vivem dentro do card de Demanda — Estratégia e Roteiros. Define estrutura, taxonomia, regras de conformidade e padrões linguísticos que os agentes de Trigger 1 e Trigger 2 devem reproduzir.
type: reference
originSessionId: f82d95b9-14b7-4d8e-8130-b14250cb3726
---
# Card de Demanda — estrutura raiz

Cada card no DB "📋 Demandas de Roteiro" tem 4 seções:

```
## Estratégia
[subpágina link → 🧠 Estratégia {Mês/Ano} — {Cliente}]

## Feedback do aprovador
[callout 📝 vazio para anotações do aprovador sobre a estratégia]

## Roteiros
[subpágina link → ✍️ Roteiros {Mês/Ano} — {Cliente}]

## Feedback dos roteiros
[callout 📝 vazio para anotações sobre os roteiros]
```

Trigger 1 cria a subpágina de Estratégia. Trigger 2 cria a subpágina de Roteiros. Os 2 callouts de feedback são pré-existentes no template do card.

# Subpágina de Estratégia — template

**Título:** `🧠 Estratégia {Mês}/{Mês+1} {Ano} — {Cliente}`
(ex: `🧠 Estratégia Maio/Jun 2026 — Dra. Larissa`)

**Cabeçalho (quote block):**
```
> **Ciclo:** dd/mm/aaaa a dd/mm/aaaa (N semanas) · M posts/semana · K conteúdos no total · **Mix XX/XX/XX** (Autoridade · Conversão · Conexão)
```

**Blocos de contexto (em sequência):**
- **Datas-chave** (lista de bullets com dia/mês · evento sazonal relevante ao período)
- **Pilares ativados** (lista numerada 1-N, derivada do perfil do cliente)
- **Territórios discursivos** (lista inline separada por · — ex: Técnica · Sensibilidade · Elegância)
- Separador horizontal `---`

**Por semana** (heading 3 toggle verde):
```
### **Semana NN · dd/mm – dd/mm — tema curto da semana** {toggle="true" color="green_bg"}
```
Tema é uma frase curta que sintetiza a narrativa daquela semana (ex: "Abertura institucional + Dia do Oftalmo", "Pós-operatório + blefaroplastia masculina").

**Por conteúdo** (callout dentro do toggle, ícone por formato):
- 🎬 Reel
- 🎠 Carrossel

Estrutura interna do callout:
```
**Conteúdo NN · {Formato} · {Camada} · {Pilar / Território / Subnicho / Sazonal}**
**Hook:** frase exata pros primeiros 3 segundos
[Para Reel] **Copy:** parágrafo de 3-5 linhas descrevendo o que o roteiro vai cobrir
[Para Carrossel] **Estrutura sugerida:** lista numerada com 9 slides (capa → desenvolvimento → CTA)
**CTA:** chamada exata, escolhida do banco de CTAs do cliente — ou "Sem CTA direto" / "Sem CTA direto, peça didático" / "Sem CTA, peça de marca"
```

A linha 1 cruza: Camada (Autoridade/Conversão/Conexão) + Pilar (do banco do cliente) ou Território (do banco discursivo) ou Subnicho (vertical específica) ou Sazonal (data-chave). Pode cruzar 2 (ex: "Território: Técnica + Elegância · Pilar: Combinações").

**Bloco final (após `---`):**

```
## Validações operacionais
- **Mix final:** N Autoridade (XX%) · N Conversão (XX%) · N Conexão (XX%) → dentro/fora do alvo XX/XX/XX.
- **Distribuição por pilar:** Pilar1 N · Pilar2 N · ...
- **Proibições respeitadas:** [lista das proibições do cliente, confirmando aderência]. Conformidade com Res. CFM 2.336/2023.
- **Territórios distribuídos:** Território1 N · Território2 N · ...

## Próximo passo
Após aprovação, mover card para **✅ Estratégia Aprovada** e abrir produção dos N roteiros (docs Cliente + Videomaker).
```

# Subpágina de Roteiros — template

**Título:** `✍️ Roteiros {Mês}/{Mês+1} {Ano} — {Cliente}`

**Cabeçalho (quote block):**
```
> **Ciclo:** dd/mm/aaaa a dd/mm/aaaa · K conteúdos (X Reels de 60-90s + Y Carrosséis) · Duração-alvo Reels: **240-280 palavras**
```

**Bloco de instrução ao revisor:**
```
**Como revisar este documento**
- Cada conteúdo está em um callout dentro da semana correspondente.
- Reels trazem hook, capa-cliente, capa-videomaker, roteiro completo, legenda + assinatura e referências.
- Carrosséis trazem capa, estrutura slide a slide, legenda + assinatura e referências. No campo "Capa videomaker" está marcado "não precisa gravar".
- Comente direto nesta página o que precisar ajustar. Quando aprovar, mover o card pai para **✅ Roteiros Aprovados** e eu gero os 2 docs no Drive (Cliente + Videomaker).
```

Separador `---`.

**Semanas** seguem mesmo formato da estratégia (heading 3 toggle verde, mesmo título/tema).

**Por conteúdo** (callout, mesmos ícones de formato):

Linha 1: `**Conteúdo NN · {Formato} · {Camada} · {Pilar / Território / Subnicho / Sazonal}**` (idêntica à da estratégia).

**Para Reel:**
```
**Hook:** frase do hook
**Capa (cliente):** texto curto que vai sobreposto na capa (estilo Notion-like, simples)
**Capa (videomaker):** descrição visual da cena (plano, ambiente, paleta, texto sobreposto, tipografia)
[Opcional] **Notas de gravação:** instruções de tom, ritmo, pausas, planos de corte
**Roteiro:** texto completo, 240-280 palavras, 1ª pessoa, voz do médico
**Legenda:**
[3-5 parágrafos curtos]

[CTA final, igual ou variação do CTA da estratégia]

[assinatura — em itálico:] *{Nome} | CRM {nnnnn} | RQE {nnnnn} ⚠️ Esse conteúdo é informativo e não substitui uma consulta com um médico especialista.*
**Referências:**
- [fonte 1: regulação, sociedade médica, paper]
- [fonte 2]
- [fonte 3]
```

**Para Carrossel:**
```
**Hook:** frase do hook
**Capa (cliente):** texto curto da capa
**Capa (videomaker):** Não precisa gravar.
**Slides:**
**Slide 01 (Capa):** texto da capa (geralmente repete o hook)
**Slide 02:** desenvolvimento — 1-3 linhas
[...até Slide 09 ou conforme estrutura definida na estratégia]
**Slide 09 (CTA):** chamada final
**Legenda:** [3-5 parágrafos] + CTA + [assinatura padrão]
**Referências:** [lista]
```

# Taxonomia padrão

**Camadas** (sempre 3, percentuais somam 100%):
- **Autoridade** — educa, posiciona como referência técnica
- **Conversão** — leva pra ação (agendar, avaliar)
- **Conexão** — humaniza, reforça vínculo, datas comemorativas

**Formatos:**
- 🎬 Reel — 60-90s, 240-280 palavras
- 🎠 Carrossel — 9 slides padrão (capa + 7 desenvolvimento + CTA)

**Eixos cruzáveis** na linha 1 do callout:
- Pilar — tema clínico recorrente do médico (ex: Blefaroplastia, Ptose, Combinações)
- Território — eixo discursivo (ex: Técnica, Sensibilidade, Elegância)
- Subnicho — vertical específica (ex: "Blefaroplastia masculina")
- Sazonal — data comemorativa (ex: "Sazonal: Dia do Oftalmologista (07/05)")

# Regras de escrita não-negociáveis

1. **Zero travessão.** (consistente com `feedback_writing_style_yan`)
2. **Toda afirmação técnica precisa de referência citada.** (CFM, sociedades médicas como CBO/SBCPO/ASOPRS, papers em revistas indexadas)
3. **Disclaimer assinado em TODA legenda:** `*{Nome} | CRM {n} | RQE {n} ⚠️ Esse conteúdo é informativo e não substitui uma consulta com um médico especialista.*`
4. **Reels 240-280 palavras.** (consistente com `feedback_roteiro_duration`)
5. **Hook nos primeiros 3 segundos** — frase curta, provocativa, baseada em mito-vs-verdade ou pergunta direta.
6. **1ª pessoa, voz do médico.** Tom firme mas acolhedor (varia por arquétipo).
7. **Conformidade Res. CFM 2.336/2023** — sem promessa de resultado, sem antes/depois, sem comparação com outras pacientes, sem urgência artificial, sem ataque a concorrente, sem banalização cirúrgica.
8. **CTAs do cliente** — usar do banco "CTAs padrão" do cliente, não inventar.
9. **Sem dancinha, sem humor, sem trends que reduzam autoridade.** (consistente com proibições padrão da agência)

# Renderização técnica no Notion

- Toggle: usar `{toggle="true" color="green_bg"}` no heading 3
- Callout: tag `<callout icon="EMOJI">` ... `</callout>`
- Indentação: conteúdo da semana indentado 1 tab dentro do toggle; conteúdo do callout indentado 1 tab dentro do callout
- CRITICAL: usar caracteres reais (newlines, emojis), nunca `\n` ou `\uXXXX` literal — Notion renderiza literal e quebra
- Pipe em `CRM {n} | RQE {n}` deve ser escapado como `\|` em sintaxe Notion-flavored markdown

# Referência viva

Card-modelo: https://www.notion.so/Roteiros-Dra-Larissa-34e627656baf80269091ead501ccc852
Subpágina Estratégia: https://www.notion.so/34e627656baf81269241ea717ffbafa2
Subpágina Roteiros: https://www.notion.so/34f627656baf81989554df36d1ce0948

Sempre que houver dúvida de formato, abrir essas 3 URLs e calibrar.
