# am-radar-noticias

Radar semanal de notícias que viram pauta. Varre ciência, regulação, imprensa e cultura dos últimos dias, filtra pelo nicho de um médico cliente e devolve um relatório com as melhores pautas, já com ângulo, formato e hook sugeridos. Disparado por routine agendada (cron) ou invocação manual.

---

## Contexto (ler antes de executar)

- **Configuração do cliente:** `radar/{slug}.md` (eixos, palavras-chave, fontes, calendário, cuidados). Sem config, abortar e dizer qual arquivo falta.
- Memórias **obrigatórias:**
  - `memory/feedback_writing_style_yan.md`: zero travessões + toda afirmação técnica com fonte
  - `memory/project_medical_marketing_agency.md`: autoridade médica acima de viralização
- Copy modules de apoio (para ângulo e hook, não precisa aplicar por inteiro):
  - `copy-modules/awareness-calibration.md`: nível de consciência da pauta
  - `copy-modules/hooks-reels.md` e `copy-modules/hooks-carrossel.md`: estilo de hook
  - `copy-modules/proof-pattern.md`: hierarquia de força de evidência
- Se a config apontar um Perfil V1 no Notion e o conector estiver disponível, ler tom, hooks favoritos e proibições do perfil. Se não estiver, seguir só com a config.

## Input esperado

- **slug** do cliente (ex: `thiago-brandao`). Se omitido e houver um único arquivo em `radar/`, usar esse.
- (opcional) **janela** em dias. Default: o que a config definir, senão 7.
- **Data de hoje:** pegar do ambiente (`date`). Todas as janelas são calculadas a partir dela.

## Fluxo de execução

### Fase 1: Preparação

1. Ler a config e as memórias acima.
2. Calcular a janela: `hoje - N dias` até `hoje`. Calendário: `hoje` até `hoje + 30 dias`.
3. **Congressos primeiro:** checar no calendário de congressos da config se algum aconteceu na janela. Se sim, as primeiras buscas são por ele (é onde saem os estudos da semana).
4. Montar o plano de busca: para **cada eixo** da config, de 3 a 5 consultas combinando palavras-chave com termos de novidade. Exemplos: `semaglutida estudo outubro 2026`, `marathon study 2026 runners`, `Anvisa PRP`, `knee osteoarthritis trial results`. Incluir o mês e o ano corrente nas consultas para puxar o que é recente.
5. **Orçamento:** de 30 a 45 buscas na varredura e até 20 na verificação. Não precisa cruzar todo eixo com toda camada: priorizar as combinações que mais rendem (congresso, regulação, imprensa grande).

### Fase 2: Varredura (WebSearch)

Rodar as buscas em 4 camadas, por eixo:

1. **Ciência:** estudos publicados na janela (priorizar periódicos listados na config).
2. **Regulação:** decisões e alertas de Anvisa, CFM, Ministério da Saúde, FDA, EMA, OMS, sociedades médicas.
3. **Imprensa:** matérias de saúde com repercussão (servem para achar a pauta; a fonte final é o estudo ou o órgão original).
4. **Cultura:** provas, atletas, famosos, tendências de rede social ligadas aos eixos.

Meta: 40 a 80 candidatos brutos no total. **Candidato = fato noticioso distinto** (vários links sobre o mesmo fato contam como um). Manter um log simples num arquivo de rascunho (título, URL, data, eixo) para auditoria.

### Fase 3: Verificação (WebFetch nos finalistas)

Para os ~20 candidatos mais promissores:

1. **Confirmar a data**, sempre pela página do periódico ou do órgão, nunca pelo resumo do WebSearch (o resumo às vezes mistura datas e estudos). Regra de janela:
   - **Data da pauta = primeira repercussão relevante dentro da janela.**
   - Estudo publicado até 6 meses antes é aceito se repercutiu na janela. Sinalizar "estudo de {mês/ano}, repercutiu em {dd/mm}".
   - Fato fora da janela pode aparecer como contexto de uma pauta, nunca como a pauta em si.
   - Fora dessas regras: vai para "Descartados por data" (Fase 7).
2. **Rastrear a fonte primária.** Matéria de imprensa sobre estudo: achar o paper (DOI, PubMed ou página do periódico). Notícia regulatória: achar a página oficial do órgão. Se não achar, rebaixar a nota de evidência e sinalizar "fonte primária não localizada".
3. **Classificar a evidência:**
   - Estudos: meta-análise ou revisão sistemática > ensaio clínico randomizado > coorte/observacional > série de casos > estudo em animal/in vitro > opinião. Preprint sempre sinalizado.
   - Não-estudos: documento oficial de órgão regulador ou sociedade médica = 3; reportagem de veículo da lista com dados próprios ou pesquisa de opinião de instituto reconhecido = 1; reportagem sem dados próprios = 0.
4. **Ler o que o estudo de fato concluiu** (abstract no mínimo). A pauta não pode exagerar o achado.

**Se o WebFetch estiver bloqueado ou indisponível:** verificar por WebSearch dirigida (título exato, DOI ou nome do ensaio), exigir **2 fontes concordantes**, marcar a pauta como "verificado por busca" e limitar a nota de Evidência a 2 quando o abstract não foi lido. Avisar no topo do relatório quais domínios bloquearam.

Nunca inventar link, DOI, número ou nome de periódico. Se não conseguiu confirmar, não entra.

### Fase 4: Pontuação

Cada candidato verificado recebe nota de 0 a 3 em:

| Critério | O que mede |
|---|---|
| **Aderência** | Quão central é para os eixos do médico (3 = coração do nicho) |
| **Timing** | Quão quente está (3 = saiu agora e está repercutindo, ou data-chave próxima) |
| **Autoridade** | Quanto permite ao médico explicar, corrigir mito ou dar contexto que o público não tem |
| **Evidência** | Força da fonte (ver Fase 3) |

**Penalidade de risco** (subtrai de 0 a 3): risco CFM, tema que induz automedicação, polêmica que expõe o médico, tema politizado ou eleitoral, sensacionalismo difícil de evitar.

Nota final = soma dos 4 critérios menos a penalidade (máximo 12). Entram no relatório as pautas com nota ≥ 7, até no máximo 12. Se houver menos de 5 com nota ≥ 7, completar com as melhores abaixo do corte e marcar "pauta de reserva".

Bônus de conexão: quando a pauta permite o médico falar da vivência própria (ex: maratonista), anotar isso no ângulo.

**Eixo sem notícia na janela:** se um eixo da config ficar zerado, pode entrar 1 "pauta perene" desse eixo (decisão regulatória recente, diretriz, dúvida recorrente do público), marcada como tal.

### Fase 5: Transformar notícia em pauta

Para cada pauta selecionada, preencher:

- **Título da pauta** (o tema, não a manchete da matéria)
- **Eixo** e **nota** (ex: `Emagrecimento · 10/12`)
- **O que aconteceu:** 2 a 3 frases, factuais, com data
- **Por que importa para o público dele:** 1 a 2 frases
- **Ângulo sugerido:** a tese que o médico defende ao comentar (desmistificar, contextualizar, alertar, traduzir o estudo, relato de corredor)
- **Camada e consciência:** Autoridade, Conversão ou Conexão + nível de consciência (`awareness-calibration.md`)
- **Formato:** Reel react (comentando a notícia na tela), Reel educativo, ou Carrossel (quando há lista, dados ou passo a passo)
- **Hook sugerido:** 2 opções de **categorias diferentes** de `hooks-reels.md` (citar a categoria entre parênteses), até 12 palavras cada, sem clickbait que fira o CFM e sem nome comercial de medicamento. Aqui são 2, não 3: o radar sugere, a skill de roteiros escolhe o hook final.
- **Fontes:** link primário (estudo/órgão) + link da matéria, se houver
- **Cuidados:** o que não pode ser dito (dose, promessa, off-label, diagnóstico de famoso)
- **Validade:** até quando a pauta está quente (ex: "usar até 18/10", "atemporal")

### Fase 6: Calendário dos próximos 30 dias

Listar as datas da config que caem entre hoje e hoje + 30 dias, **confirmando a data exata do ano corrente** via busca (provas mudam de data). Para cada uma: data, evento, sugestão de pauta em uma linha.

### Fase 7: Relatório final (saída no chat)

Formato em Markdown, nesta ordem:

```
# Radar de pautas · {Nome do médico} · semana {dd/mm} a {dd/mm/aaaa}

**Resumo:** {N} candidatos analisados · {M} verificados · {K} pautas selecionadas

## Top 3 da semana
{as 3 maiores notas, com todos os campos da Fase 5}

## Demais pautas
### {Eixo}
{pautas restantes agrupadas por eixo, todos os campos da Fase 5}

## Calendário dos próximos 30 dias
| Data | Evento | Pauta sugerida |

## No radar (monitorar)
{até 5 itens que ainda não viraram pauta: estudo anunciado mas não publicado, decisão regulatória pendente, tendência começando. Uma linha cada, com link.}

## Descartados por data, mas vale saber
{até 5 fatos relevantes que caíram fora da janela. Uma linha cada: data, fato, link. Fatos envolvendo pessoa real em situação de saúde ficam fora desta lista.}
```

### Fase 8: Revisão antes de entregar

Checklist obrigatório:

- [ ] Zero travessões (`—` e `–`) em todo o relatório. Usar vírgula, ponto, dois-pontos ou parênteses.
- [ ] Toda afirmação técnica tem link de fonte
- [ ] Todos os links foram efetivamente abertos ou retornados pela busca (nenhum inventado)
- [ ] Todas as pautas respeitam a regra de janela da Fase 3 (estudos anteriores sinalizados)
- [ ] Estudos em animal, in vitro e preprints estão sinalizados
- [ ] Nenhum hook promete resultado, cria urgência artificial ou sugere automedicação
- [ ] Nenhuma pauta diagnostica ou especula sobre a saúde de pessoa real

## Abort

- Config `radar/{slug}.md` inexistente: avisar e parar.
- WebSearch indisponível: avisar no log e parar (não produzir relatório com conhecimento de memória, porque vira notícia velha).
- WebFetch indisponível **não** é abort: seguir o fallback de verificação por busca da Fase 3.
