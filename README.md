# Agência A.M. — Sistema de Roteiros

Repositório do sistema automatizado de geração de estratégia, roteiros e onboarding da agência de marketing médico do Yan. Serve como **source of truth** pras routines remotas que rodam em [claude.ai/code/routines](https://claude.ai/code/routines).

## Estrutura

```
agencia-am/
├── skills/                  # Skills (slash commands) que as routines invocam
│   ├── AM_cliente.md        # Trigger 0 — onboarding de novo cliente
│   ├── AM_estrategia.md     # Trigger 1 — gera/refaz subpágina de Estratégia
│   ├── AM_roteiros.md       # Trigger 2 — gera/refaz subpágina de Roteiros
│   └── AM_radar_noticias.md # Radar semanal de notícias que viram pauta
│
├── radar/                   # Configuração do radar de pautas, um arquivo por médico
│   └── thiago-brandao.md    # Performance, emagrecimento, dor, regenerativa, corrida
│
├── copy-modules/            # Squad de copywriting encarnada em módulos
│   ├── big-idea-mensal.md           # Todd Brown (E5 Method)
│   ├── awareness-calibration.md     # Eugene Schwartz
│   ├── proof-pattern.md             # Claude Hopkins
│   ├── hooks-reels.md               # John Carlton
│   ├── hooks-carrossel.md           # Gary Bencivenga
│   ├── slippery-slide.md            # Joe Sugarman
│   └── ctas-medicos.md              # Dan Kennedy (versão soft)
│
└── memory/                  # Memórias do projeto (regras, padrões, decisões)
    ├── reference_am_estrategia_roteiros_template.md  # Gold standard (Larissa DEM-10)
    ├── reference_am_roteiro_output_folder.md         # Pasta Drive + naming
    ├── feedback_am_kanban_visual_convention.md       # Cinza = robô, colorido = humano
    ├── feedback_writing_style_yan.md                 # Zero travessões + referências
    ├── feedback_notion_strategy_formatting.md        # Sintaxe Notion (toggle, callout)
    ├── feedback_roteiro_duration.md                  # Reels 60-90s, 240-280 palavras
    ├── feedback_gdocs_roteiro_format.md              # Formato dos GDocs finais
    ├── feedback_kanban_status_protocol.md            # Protocolo de movimentação
    ├── reference_client_onboarding_inputs.md         # Frame.io + Drive de estáticos
    ├── reference_onboarding_strategic_quiz.md        # Quiz 14 perguntas
    ├── reference_google_account.md                   # yan.sarmento@gmail.com
    └── project_medical_marketing_agency.md           # Contexto da agência
```

## Arquitetura do sistema

```
DB Notion "📋 Demandas de Roteiro"
   │
   │  [humano move status pra "🤖 Gerando Estratégia"]
   ↓
Notion Automation → webhook → Claude Code routine "am-estrategia-gerar"
   │
   │  [routine clona este repo + invoca skills/AM_estrategia.md]
   ↓
Skill executa:
  - Lê card de Demanda + perfil do cliente (Notion MCP)
  - Aplica copy-modules/big-idea-mensal.md (Big Idea do mês)
  - Aplica copy-modules/awareness-calibration.md (níveis de consciência)
  - Constrói calendário com 15 conteúdos
  - Cria subpágina de Estratégia
  - Atualiza status pra "🧠 Estratégia p/ Revisar"
```

Mesma lógica pros outros 3 triggers (`am-estrategia-refazer`, `am-roteiros-gerar`, `am-roteiros-refazer`).

## 4 routines (criadas em claude.ai/code/routines)

| Routine | Disparo (Status do card) | Skill | Modo |
|---|---|---|---|
| `am-estrategia-gerar` | `🤖 Gerando Estratégia` | `AM_estrategia.md` | gerar |
| `am-estrategia-refazer` | `🤖 Refazendo Estratégia` | `AM_estrategia.md` | refazer |
| `am-roteiros-gerar` | `🤖 Gerando Roteiros` | `AM_roteiros.md` | gerar |
| `am-roteiros-refazer` | `🤖 Refazendo Roteiros` | `AM_roteiros.md` | refazer |

## Radar de pautas (routine agendada)

| Routine | Disparo | Skill | Config |
|---|---|---|---|
| `am-radar-thiago` ([`trig_01R4y5xnpadkYRASqcHzuYLu`](https://claude.ai/code/triggers/trig_01R4y5xnpadkYRASqcHzuYLu)) | Cron `46 16 * * 0` America/Sao_Paulo (domingo 16h46) | `AM_radar_noticias.md` | `radar/thiago-brandao.md` |

Varre ciência, regulação, imprensa e cultura dos últimos 7 dias, verifica a fonte primária, pontua cada notícia (aderência, timing, autoridade, evidência, menos risco CFM) e entrega no log da routine as melhores pautas com ângulo, formato, hook e cuidados, mais o calendário dos próximos 30 dias. O relatório sai no log da sessão em [claude.ai/code/sessions](https://claude.ai/code/sessions). A routine não usa conectores. Para ela ler sempre a versão mais nova da skill, conectar o repo `yansarmento-oss/agencia-am` na UI da routine (mesmo passo das outras). Para adicionar outro médico: copiar `radar/thiago-brandao.md` com o novo slug, ajustar eixos e fontes, e criar uma routine apontando para o slug.

## Edição

Editar arquivo local → `git add . && git commit -m "..." && git push` → routines pegam a versão nova na próxima execução.

## Conectores Notion necessários

As routines precisam ter o conector **Notion** habilitado pra ler/escrever no DB Demandas e DB Clientes do workspace do Yan.

## Histórico

- 2026-05-08: Squad de copy reformulada com 7 módulos (Hopkins + Schwartz no comando, Carlton/Sugarman/Bencivenga/Kennedy especialistas, Todd Brown como estrategista de Big Idea).
- Card-modelo de validação: Dra. Larissa DEM-10 (Maio/Jun 2026) e DEM-14 (Junho/Jul 2026, ciclo de teste).
