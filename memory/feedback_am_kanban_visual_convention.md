---
name: AM Kanban — convenção visual robô vs humano
description: Padrão visual nos status do DB "📋 Demandas de Roteiro" para sinalizar quem é dono da ação. Cinza + 🤖 = obrigação do robô (Claude). Colorido = obrigação humana (SM/aprovador/médico). Aplicado pra que a equipe leia o board sem precisar de legenda.
type: feedback
originSessionId: f82d95b9-14b7-4d8e-8130-b14250cb3726
---
**Regra:**
- **Status cinza + ícone 🤖 no nome** = ação pendente do robô (Claude). Humano não precisa fazer nada, é só esperar.
- **Status colorido (qualquer cor exceto cinza)** = ação pendente do humano (SM, aprovador, médico, videomaker, editor).

**Why:** Yan precisa que a equipe (SMs, aprovadores) bata o olho no board e saiba na hora se tá esperando o robô ou se tem algo pra fazer. Sem legenda, sem treinamento. Cinza = "não é com você". Colorido = "é com você".

**How to apply:** Toda vez que adicionar/renomear status no DB Demandas (ou em qualquer outro DB do Sistema A.M. com fluxo Claude+humano), seguir essa convenção. Status transitórios do robô (gerando, refazendo) sempre cinza + 🤖. Estados de espera humana (revisar, aprovar, gravar, publicar) sempre coloridos.

**Decisão relacionada:** o prompt dos agentes do Sistema A.M. de Roteiros vive como **skill do Claude Code** (não arquivo solto, não página Notion). Discutido previamente em sessão com o Chief de Copywriter, validado.
