# onboard-cliente

Onboarding automático de novo cliente da agência médica do Yan. Recebe respostas do Notion Form "Onboarding de Cliente", enriquece com pesquisa + análise de ativos, preenche a DB de Clientes, gera perfil completo e emite diagnóstico de Brand Kit.

---

## Contexto (ler antes de executar)

- DB Clientes: `64d4f465-a1c2-44b6-9a14-c98331c10027` (data source `collection://9a66bcde-df8a-4286-bd25-e7e592660e11`)
- Parent page agência: "A.M. - Sistema de Roteiros" (`341627656baf8085a6f8fa2c18a1aad8`)
- Memórias relevantes: `feedback_writing_style_yan.md` (zero travessões + referências), `reference_client_onboarding_inputs.md` (frame.io + Drive), `project_medical_marketing_agency.md`

## Input esperado

O usuário aciona com um (ou mais) de:
- Link da resposta do Notion Form
- Upload direto dos dados do form colados no chat
- Nome do cliente pra buscar respostas pendentes

Se múltiplos clientes em paralelo ("rode pra Larissa e Dr. Y"), processar sequencialmente cada um e devolver status por cliente.

## Fluxo de execução

### Fase 1 — Coleta e validação

1. Ler resposta do form (18 perguntas cobrindo: identificação, negócio, ativos, contexto opcional).
2. Validar campos obrigatórios:
   - Nome, Especialidade, Localização, Instagram, WhatsApp, SM responsável, **Assinatura padrão** (contém CRM + RQE + sobrenome completo, parsear dali em vez de pedir campos separados), Produto-âncora, link pasta Drive de estáticos
   - Se Instagram < 20 posts OU sem site → transcrições frame.io coladas no campo texto **obrigatórias**
3. Se faltar algo crítico: parar, listar gaps pro SM e encerrar sem criar o cliente.

### Fase 2 — Research e análise de ativos

Em paralelo quando possível:

**A) Instagram + site**
- Se IG público: buscar últimos 20 posts (via WebFetch ou pedir ao SM os prints). Extrair: bordões recorrentes, analogias, tom, hooks, CTAs, hashtags.
- Se site: WebFetch → extrair bio, serviços, linguagem.

**B) Pasta Drive de estáticos**
- Listar arquivos (mcp__google-workspace__list_drive_items).
- Baixar 8-12 posts representativos.
- Extrair: paleta (cores primárias/secundárias), tipografia, mood visual, consistência da identidade.

**C) Transcrições frame.io (se fornecidas)**
- Ler campo "Transcrições frame.io" (texto colado pelo SM, múltiplos vídeos separados por `---`).
- Extrair: voz real do médico, vocabulário técnico vs acessível, analogias favoritas, estrutura narrativa.

### Fase 3 — Inferência dos 22 campos qualitativos

Com base nos dados coletados, preencher:

**Identidade/voz:** Tom de voz, Bordões, Analogias, Hooks favoritos, CTAs padrão, Tagline, Arquétipo de marca (Jung), Hashtags de marca

**Público:** Público-alvo (persona detalhada), Nível de consciência, Subnicho

**Visual:** Cores primárias, Cores secundárias, Tipografia, Mood visual

**Governança:** Temas proibidos (gerar combinando CFM + especialidade + declaração do médico), Proibições (multi-select da DB), Mix autoridade/conversão (sugestão inicial baseada em estágio da marca)

Cada campo qualitativo deve citar a fonte da inferência em comentário interno (ex: "Tom: baseado em 4 transcrições frame.io — didático-afetivo").

Se algum campo não for inferível com confiança: marcar como `null` + listar no output "campos pra confirmar com o médico".

### Fase 4 — Diagnóstico Brand Kit

Avaliar os estáticos + logo + consistência e definir Status Brand Kit:

- **🟢 Existente**: logo consistente em 100% dos estáticos + paleta clara (≤3 cores primárias recorrentes em ≥80% dos posts) + tipografia reconhecível. Parte pra roteiro.
- **🟡 Parcial**: tem logo mas visual varia (paleta inconsistente ou tipografia ad hoc). Roteiro rola, recomendar branding book em paralelo.
- **🔴 A criar**: sem logo OU cada post parece marca diferente. Bloqueio pra escalar conteúdo sem branding book.

Além disso, avaliar gate de **posicionamento**:
- Pilares de conteúdo identificáveis (3-5 temas recorrentes)? Se não → recomendar sessão de posicionamento antes de abrir briefing.

### Fase 5 — Criação da página na DB

1. Criar page na DB Clientes (mcp__claude_ai_Notion__notion-create-pages com parent `data_source_id: 9a66bcde-df8a-4286-bd25-e7e592660e11`) com todos os 34 campos preenchidos ou `null`.
2. Corpo da página = **Perfil V1** em markdown estruturado:
   ```
   # [Nome do médico]

   ## Identidade
   - CRM, RQE, Especialidade, Localização
   - Assinatura padrão

   ## Posicionamento
   - Subnicho, Tagline, Produto-âncora
   - Público-alvo (persona detalhada)
   - Nível de consciência

   ## Voz e linguagem
   - Tom, Arquétipo, Bordões, Analogias, Hooks favoritos, CTAs padrão

   ## Identidade visual
   - Paleta, Tipografia, Mood
   - Status Brand Kit: [🟢/🟡/🔴] + justificativa

   ## Governança
   - Temas proibidos, Proibições

   ## Pilares de conteúdo sugeridos
   - [3-5 pilares identificados]

   ## Diagnóstico de onboarding
   - [pronto pra roteiro / precisa branding book / precisa posicionamento]
   - Próximo passo recomendado

   ## Gaps e campos a confirmar com o médico
   - [lista]

   ## Fontes usadas
   - Instagram: [link]
   - Estáticos Drive: [link]
   - Transcrições frame.io: [link]
   ```

### Fase 6 — Output final no chat

Devolver pro Yan/SM:

```
✅ Onboarding [Nome] finalizado
Status: 🟢/🟡/🔴 [diagnóstico em 1 linha]
Link: [URL da página Notion criada]
Próximo passo: [abrir briefing / criar branding book / sessão de posicionamento]
Gaps pendentes: [lista curta ou "nenhum"]
```

Se múltiplos clientes: um bloco por cliente.

## Regras de qualidade (não negociáveis)

- **Zero travessões (—)** em qualquer texto gerado. Usar vírgula, ponto, ou "|" no lugar.
- **Toda afirmação técnica/científica** que entrar no perfil precisa de referência citada (CFM, SBACV, PubMed). Ver memory `feedback_writing_style_yan.md`.
- **Não inventar** dados do médico. Se não achou, marcar `null` e listar no gaps.
- **Não pular diagnóstico Brand Kit** mesmo se todos os campos preenchidos. Diagnóstico é o output mais valioso pro Yan.

## Quando parar e perguntar ao Yan

- Cliente já existe na DB (checar por Nome antes de criar)
- Conflito entre dado do form e dado público (ex: form diz CRM X mas IG mostra CRM Y)
- Transcrições sugerem tom MUITO diferente do que SM declarou
- Brand Kit 🔴 mas cliente já tem demandas ativas no Kanban (decisão de continuar ou pausar é do Yan)

## Pilotos / testes

Primeiro cliente piloto: **Dra. Larissa** (decisão Yan, 2026-04-13).
