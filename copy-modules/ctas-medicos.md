# CTAs Médicos — Dan Kennedy (versão soft)

> Módulo de execução. Lido pela `/AM_roteiros` ao definir o CTA de cada conteúdo.

## Princípio

Dan Kennedy é o cara do CTA específico. **Mas** o Kennedy original é direct response B2C agressivo (urgência, escassez, "compre agora"). Aqui ele entra **filtrado** — mantemos o rigor da clareza, eliminamos o hype.

A regra do CTA médico é simples: **ação clara, sem urgência fake, sem promessa de resultado.**

> "A confused mind says no. So does an offended one." — Kennedy adaptado pra médico

## 4 princípios não-negociáveis

### 1. Especificidade — UM próximo passo, não três

❌ "Curta, salva, comenta, compartilha, segue, manda pra alguém"
✅ "Agende uma avaliação. WhatsApp no topo do perfil."

Cérebro paralisa com 5 opções. Decide com 1.

### 2. Clareza do canal — nomeia o canal exato

❌ "Entra em contato"
✅ "WhatsApp 71 99932-3292"
✅ "Link da agenda na bio"
✅ "Comenta AVALIAÇÃO que eu te oriento"

### 3. Sem urgência fake

❌ "Vagas limitadas! Apenas hoje!"
❌ "Últimas 3 avaliações disponíveis essa semana"
❌ "Agenda fechando, garanta a sua"

✅ "Quando quiser dar o próximo passo"
✅ "Sem pressa. Avaliação quando você se sentir pronta."
✅ "Decisão importante pede tempo."

CFM proíbe urgência artificial. E médico bom **não tem agenda apertada por marketing**, tem agenda apertada por demanda real. Comunicar isso aumenta autoridade.

### 4. Sem promessa de resultado

❌ "Agende e recupere a aparência jovem"
❌ "Fim das pernas pesadas em 30 dias"

✅ "Agende uma avaliação personalizada"
✅ "É na avaliação individual que se entende seu caso"
✅ "Entenda se [procedimento] é indicado pra você"

A promessa é de **método** (avaliação rigorosa), não de resultado (transformação garantida).

## Banco de CTAs por intenção

### A — Avaliação direta (Conversão)

Pra Reels/Carrosséis de fundo de funil (Most Aware, Product Aware):

- "Agende uma avaliação. WhatsApp no topo do perfil."
- "Comenta AVALIAÇÃO que eu te oriento."
- "Link da agenda na bio. Avaliação presencial, com tempo dedicado."
- "Quando for a hora da sua avaliação, o WhatsApp tá no topo do perfil."
- "Se você se reconheceu, agende uma avaliação especializada."
- "Agende sua avaliação. Cuidar do seu olhar começa pela informação correta."

### B — Convite suave (Conversão indireta)

Pra Reels/Carrosséis de meio de funil (Solution Aware):

- "Antes de tratar, avalie. A decisão certa começa por olhar com atenção."
- "Pergunte ao seu cirurgião qual técnica faz sentido pro seu caso."
- "Se você se identificou, vale uma avaliação."
- "Em uma avaliação individual, isso fica claro."
- "Vale conversar com um especialista — preferencialmente um da especialidade."

### C — Salvar/Compartilhar (Autoridade pura)

Pra conteúdos didáticos sem intenção comercial:

- "Salva esse vídeo pra revisitar."
- "Salva esse carrossel pra quando alguém te perguntar."
- "Manda pra alguém que precisa ver."
- "Comenta se faz sentido pra você."

### D — Sem CTA direto (peça didática ou de marca)

Pra Reels/Carrosséis institucionais ou manifesto:

- `Sem CTA direto, peça didático.`
- `Sem CTA, peça de marca.`
- `Sem CTA — encerramento de ciclo.`

Esses casos são intencionais: o conteúdo é puro posicionamento de marca, sem chamada. Eles são **necessários** num ciclo (não tudo precisa converter).

## Mapa CTA × Nível de Consciência

| Nível | CTA padrão |
|---|---|
| Unaware | Sem CTA direto. Peça didática. |
| Problem Aware | Salvar/revisitar OU "se você se identificou, vale avaliação" |
| Solution Aware | "Em avaliação individual fica claro" / "Pergunte ao especialista" |
| Product Aware | "Agende avaliação especializada" |
| Most Aware | "Link da agenda na bio" / "WhatsApp no topo" / "Agende sua avaliação" |

## Como usar o campo "CTAs padrão" do Perfil V1

Cada cliente tem campo `CTAs padrão` no DB Clientes (banco de chamadas que **funcionam pra ele**). A `/AM_roteiros` deve:

1. Ler o banco de CTAs do cliente
2. Escolher o CTA mais adequado ao nível de consciência do conteúdo
3. **NÃO inventar CTA novo** sem motivo

Quando criar CTA novo (ex: campanha sazonal), considerar adicionar ao banco do cliente após aprovação.

## Estrutura técnica do CTA no Reel

O CTA fica nos últimos 5-10 segundos do Reel. Padrão:

```
[Peak — frase memorável]
[Linha de respiração / pausa]
[CTA — 1-2 frases curtas]
```

Exemplo (roteiro 01 Larissa):
```
Peak: "Não existe cirurgia simples. Existe cirurgia bem feita."
CTA: "Quanto mais cedo a circulação é olhada com atenção, mais simples costuma ser resolver. Se isso fez sentido, salva esse vídeo."
```

(esse é CTA C — salvar; coerente com nível Problem Aware)

## Estrutura técnica do CTA no Carrossel

O CTA vai no **último slide** (geralmente Slide 9 num carrossel padrão de 9 slides).

Padrão:
```
**Slide 09 (CTA):** [chamada concreta] [canal específico]
```

Exemplo (carrossel 17 Reinaldo):
```
**Slide 09 (CTA):** Se você se reconheceu em 2 ou mais desses pontos, vale uma consulta agora. 📱 WhatsApp no topo do perfil: 71 99932-3292.
```

## CTA na legenda (sempre presente)

Mesmo Reels com "Sem CTA direto" no roteiro têm CTA na **legenda**. A legenda é onde a pessoa que parou pra ler vai pegar o gancho:

Padrão:
```
[Texto da legenda — 3-5 parágrafos]

[CTA — 1 frase clara]

*{Assinatura padrão CRM/RQE/disclaimer}*
```

## Anti-padrões (NÃO fazer)

- "Compre agora!" (não vendemos produto, vendemos avaliação)
- "Vagas limitadas / últimas vagas / agenda fechando" (urgência fake banida)
- "Garanta a sua transformação" (promessa de resultado banida)
- CTA com 5 ações ("curta, comenta, salva, segue, manda pra alguém")
- "Clica no link da bio" sem contexto (frio, sem hook emocional)
- "Sou o melhor da cidade" / "ninguém faz como eu" (autopromoção banida CFM)
- Pressão psicológica ("se você não cuidar agora, vai se arrepender")

## Critérios de qualidade Kennedy-soft (checklist)

Para cada CTA gerado, validar:

- [ ] **UMA ação clara** (não 3-5)
- [ ] **Canal específico nomeado** (WhatsApp/link/comenta palavra)
- [ ] **Zero urgência artificial** (sem "vagas limitadas", "agenda fechando")
- [ ] **Zero promessa de resultado** (foco em método)
- [ ] **Cabe no nível de consciência** do conteúdo (ver tabela)
- [ ] **Vem do banco do cliente** (Perfil V1) ou justifica criação nova
- [ ] **Voz do médico** (não tom de vendedor)
- [ ] **Compatível com Big Idea do mês**

## Banco crescente

Toda CTA aprovada em revisão **alimenta o banco** de CTAs do cliente no DB Clientes (campo `CTAs padrão`). A `/AM_roteiros` consulta o banco antes de usar/criar CTA.
