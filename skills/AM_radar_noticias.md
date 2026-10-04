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
3. Montar o plano de busca: para **cada eixo** da config, de 3 a 5 consultas combinando palavras-chave com termos de novidade. Exemplos: `semaglutida estudo outubro 2026`, `marathon study 2026 runners`, `Anvisa PRP`, `knee osteoarthritis trial results`. Incluir o mês e o ano corrente nas consultas para puxar o que é recente.

### Fase 2: Varredura (WebSearch)

Rodar as buscas em 4 camadas, por eixo:

1. **Ciência:** estudos publicados na janela (priorizar periódicos listados na config).
2. **Regulação:** decisões e alertas de Anvisa, CFM, Ministério da Saúde, FDA, EMA, OMS, sociedades médicas.
3. **Imprensa:** matérias de saúde com repercussão (servem para achar a pauta; a fonte final é o estudo ou o órgão original).
4. **Cultura:** provas, atletas, famosos, tendências de rede social ligadas aos eixos.

Meta: 40 a 80 candidatos brutos no total. Anotar para cada um: título, URL, data, eixo.

### Fase 3: Verificação (WebFetch nos finalistas)

Para os ~20 candidatos mais promissores:

1. **Confirmar a data.** Fora da janela: descartar (exceto evento futuro do calendário).
2. **Rastrear a fonte primária.** Matéria de imprensa sobre estudo: achar o paper (DOI, PubMed ou página do periódico). Notícia regulatória: achar a página oficial do órgão. Se não achar, rebaixar a nota de evidência e sinalizar "fonte primária não localizada".
3. **Classificar a evidência:** meta-análise ou revisão sistemática > ensaio clínico randomizado > coorte/observacional > série de casos > estudo em animal/in vitro > opinião. Preprint sempre sinalizado.
4. **Ler o que o estudo de fato concluiu** (abstract no mínimo). A pauta não pode exagerar o achado.

Nunca inventar link, DOI, número ou nome de periódico. Se não conseguiu confirmar, não entra.

### Fase 4: Pontuação

Cada candidato verificado recebe nota de 0 a 3 em:

| Critério | O que mede |
|---|---|
| **Aderência** | Quão central é para os eixos do médico (3 = coração do nicho) |
| **Timing** | Quão quente está (3 = saiu agora e está repercutindo, ou data-chave próxima) |
| **Autoridade** | Quanto permite ao médico explicar, corrigir mito ou dar contexto que o público não tem |
| **Evidência** | Força da fonte (ver Fase 3) |

**Penalidade de risco** (subtrai de 0 a 3): risco CFM, tema que induz automedicação, polêmica que expõe o médico, sensacionalismo difícil de evitar.

Nota final = soma dos 4 critérios menos a penalidade (máximo 12). Entram no relatório as pautas com nota ≥ 7, até no máximo 12. Se houver menos de 5 com nota ≥ 7, completar com as melhores abaixo do corte e marcar "pauta de reserva".

Bônus de conexão: quando a pauta permite o médico falar como maratonista (vivência própria), anotar isso no ângulo.

### Fase 5: Transformar notícia em pauta

Para cada pauta selecionada, preencher:

- **Título da pauta** (o tema, não a manchete da matéria)
- **Eixo** e **nota** (ex: `Emagrecimento · 10/12`)
- **O que aconteceu:** 2 a 3 frases, factuais, com data
- **Por que importa para o público dele:** 1 a 2 frases
- **Ângulo sugerido:** a tese que o médico defende ao comentar (desmistificar, contextualizar, alertar, traduzir o estudo, relato de corredor)
- **Camada e consciência:** Autoridade, Conversão ou Conexão + nível de consciência (`awareness-calibration.md`)
- **Formato:** Reel react (comentando a notícia na tela), Reel educativo, ou Carrossel (quando há lista, dados ou passo a passo)
- **Hook sugerido:** 2 opções, até 12 palavras cada, sem clickbait que fira o CFM
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
```

### Fase 8: Revisão antes de entregar

Checklist obrigatório:

- [ ] Zero travessões (`—` e `–`) em todo o relatório. Usar vírgula, ponto, dois-pontos ou parênteses.
- [ ] Toda afirmação técnica tem link de fonte
- [ ] Todos os links foram efetivamente abertos ou retornados pela busca (nenhum inventado)
- [ ] Todas as datas das notícias estão dentro da janela
- [ ] Estudos em animal, in vitro e preprints estão sinalizados
- [ ] Nenhum hook promete resultado, cria urgência artificial ou sugere automedicação
- [ ] Nenhuma pauta diagnostica ou especula sobre a saúde de pessoa real

## Abort

- Config `radar/{slug}.md` inexistente: avisar e parar.
- WebSearch indisponível: avisar no log e parar (não produzir relatório com conhecimento de memória, porque vira notícia velha).
