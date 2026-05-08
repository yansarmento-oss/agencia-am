# Proof Pattern — Claude Hopkins

> Módulo tático. Lido pela `/AM_roteiros` em todo conteúdo que faz afirmação técnica.

## Princípio (Scientific Advertising, 1923)

> "The man who cannot explain in detail why his product is good is not the man we want to advertise it." — Claude Hopkins

Toda afirmação técnica de saúde precisa ser **passível de citação**. Sem fonte, é palpite. Com fonte, é autoridade. **Esta regra é não-negociável** — combina conformidade CFM 2.336/2023 com a regra do Yan: zero afirmação técnica sem referência.

## Hierarquia de fontes (em ordem de peso)

### Nível 1 — Regulação oficial brasileira

**Fontes prioritárias:**
- Resoluções do **Conselho Federal de Medicina (CFM)** — regulação publicitária e prática
- Pareceres dos **Conselhos Regionais de Medicina (CRMs)**
- **ANVISA** — regulação de produtos, procedimentos
- **Ministério da Saúde** — diretrizes nacionais

**Como citar:**
- "Resolução CFM nº 2.336/2023, art. 3 e 4"
- "Parecer CRM-SP nº X/2024"

**Quando usar:** sempre que tocar em compliance, publicidade médica, prática autorizada.

### Nível 2 — Sociedades médicas brasileiras (especialidade do cliente)

Cada especialidade tem sua sociedade, e elas publicam **diretrizes** e **posicionamentos**:

| Especialidade | Sociedade |
|---|---|
| Cirurgia Vascular | SBACV (Sociedade Brasileira de Angiologia e Cirurgia Vascular) |
| Cardiologia | SBC (Sociedade Brasileira de Cardiologia) |
| Dermatologia | SBD (Sociedade Brasileira de Dermatologia) |
| Endocrinologia | SBEM (Sociedade Brasileira de Endocrinologia e Metabologia) |
| Ginecologia | FEBRASGO |
| Oftalmologia | CBO (Conselho Brasileiro de Oftalmologia) |
| Oftalmoplástica | SBCPO (Sociedade Brasileira de Cirurgia Plástica Ocular) |
| Ortopedia | SBOT |
| Pediatria | SBP |
| Urologia | SBU |
| Estética | SBCD (dermatologia) ou SBCP (cirurgia plástica) |

**Como citar:**
- "SBACV. Diretrizes sobre doença arterial periférica. J Vasc Bras. 2023."
- "CBO. Classificação funcional de ptose palpebral."

**Quando usar:** sempre que afirmar um critério clínico, indicação cirúrgica, classificação, protocolo.

### Nível 3 — Sociedades internacionais consagradas

Pra reforço técnico ou quando não há equivalente nacional:

- **ASOPRS** (American Society of Ophthalmic Plastic and Reconstructive Surgery)
- **AHA** (American Heart Association)
- **AAD** (American Academy of Dermatology)
- **WHO/OMS**
- **Cochrane Collaboration** (para revisões sistemáticas)

**Como citar:**
- "ASOPRS Position Statement on Laser-Assisted Eyelid Surgery"
- "Cochrane Database Syst Rev. 2012;(11):CD003230 — Pittler & Ernst"

### Nível 4 — Papers em revistas indexadas (PubMed/SciELO)

Pra afirmações específicas (procedimento, técnica, eficácia comparativa):

**Formato Vancouver (padrão da agência):**
- "Patel BC, Malhotra R. Upper eyelid blepharoplasty: techniques and recovery. Curr Opin Ophthalmol, 2017."
- "Mendelson BC, Wong CH. CO2 laser-assisted blepharoplasty. Plast Reconstr Surg, 2014."

**Quando usar:** sempre que afirmar dado quantitativo (prevalência, eficácia, taxa de complicação) ou comparativa entre técnicas.

### Nível 5 — Documentos governamentais e estatísticos

- IBGE, DATASUS pra dados epidemiológicos brasileiros
- Boletins do Ministério da Saúde

## Quando exigir fonte (regra prática)

**Toda frase que contém um destes elementos precisa ter fonte citada:**

1. **Número específico** ("10-20% dos brasileiros acima de 60 anos...")
2. **Dado epidemiológico** ("prevalência", "incidência")
3. **Eficácia comparativa** ("X é mais eficaz que Y")
4. **Indicação clínica** ("indicado para casos de...")
5. **Protocolo padronizado** ("recomenda-se anestesia local + sedação...")
6. **Classificação técnica** ("ptose funcional vs estética")
7. **Mecanismo fisiológico não-óbvio** ("o álcool desencadeia X via Y")
8. **Comparação técnica entre opções** ("laser CO2 vs bisturi frio")

**Frases que NÃO precisam de fonte:**

- Opinião declarada ("eu acredito que...", "minha experiência mostra que...")
- Cena de consultório ("a paciente me disse que...")
- Princípio amplamente aceito sem disputa ("o coração bombeia sangue")
- Analogia didática ("é como pintar uma parede com infiltração")
- Manifesto pessoal ("escolhi essa especialidade porque...")

## Onde citar (formato no Reel/Carrossel)

**Em Reels:** referência fica em campo separado no fim do roteiro:
```
**Referências:**
- Resolução CFM nº 2.336/2023, art. 3 e 4
- SBCPO, recomendações sobre indicação cirúrgica
- Pittler MH, Ernst E. Cochrane 2012;(11):CD003230
```

Não embute referência no meio do roteiro falado (quebra a fluidez). Mas o ROTEIRISTA precisa ter a referência na mão pra defender a frase.

**Em Carrosséis:** referência pode ir no penúltimo slide ou rodapé:
- Slide 9 (CTA) | Rodapé pequeno: "Refs: SBACV 2023; ASOPRS Position 2021"
- OU campo "Referências" abaixo da legenda

**Em Legendas:** sempre listar referências no fim, em itálico ou rodapé:
```
*Referências:*
- *SBACV. Diretrizes sobre DAP. J Vasc Bras 2023.*
- *Resolução CFM 2.336/2023, art. 3.*
```

## Padrão de assinatura obrigatório

Toda legenda termina com:

```
*{Nome completo do médico} \| CRM {número} \| RQE {número}*
*⚠️ Esse conteúdo é informativo e não substitui uma consulta com um médico especialista.*
```

(Pipe escapado como `\|` em Notion-flavored markdown)

Múltiplos RQEs: listar todos (ex: Dr. Reinaldo tem 3).

## Anti-padrões

- "Estudos mostram que..." — sem citar qual estudo. **Banido.**
- "É comprovado cientificamente..." — sem fonte. **Banido.**
- "A maioria dos médicos concorda..." — opinião disfarçada de dado. **Banido.**
- "Pesquisas indicam..." — vago, sem fonte. **Banido.**
- Citar fonte que não existe ou não foi consultada — **gravíssimo. Hopkins se vira no túmulo.**

## Checklist de prova (rodar em todo conteúdo)

- [ ] Toda afirmação técnica tem fonte associada?
- [ ] Fontes citadas existem (não inventadas)?
- [ ] Hierarquia respeitada (CFM > sociedade BR > internacional > paper)?
- [ ] Formato Vancouver respeitado em papers?
- [ ] Referências listadas no fim do conteúdo?
- [ ] Disclaimer assinado em toda legenda?
- [ ] CRM e RQE visíveis?
- [ ] Nenhum "estudos mostram" / "é comprovado" sem citação?

Se qualquer item falhar: a Copy Chief pode rejeitar o conteúdo no review.
