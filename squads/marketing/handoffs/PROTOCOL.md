# Protocolo de Comunicação Entre Agentes — Marketing Squad

## Conceito

Os agentes se comunicam através de **arquivos de handoff** — documentos estruturados que um agente escreve e o próximo lê ao ser ativado. O `@marketing-orchestrator` é o roteador central.

## Fluxo de Comunicação

```
AGENTE A (envia)
    │
    ├── Escreve handoff em: squads/marketing/handoffs/{agente-destino}/inbox.md
    │
ORCHESTRATOR (roteia)
    │
    ├── Notifica agente B que tem mensagem
    │
AGENTE B (recebe)
    │
    └── Lê inbox → executa → responde ou encaminha para próximo
```

## Estrutura de Diretório

```
squads/marketing/handoffs/
  ├── traffic/
  │   └── inbox.md    ← mensagens para @traffic-manager
  ├── design/
  │   └── inbox.md    ← mensagens para @creative-director
  ├── copy/
  │   └── inbox.md    ← mensagens para @chief-copywriter
  ├── orchestrator/
  │   └── inbox.md    ← relatórios para @marketing-orchestrator
  └── log/
      └── communication-log.md  ← histórico de todas as comunicações
```

## Formato de Handoff

```markdown
---
FROM: {agente-remetente}
TO: {agente-destinatário}
DATE: {data}
PRIORITY: {alta|média|baixa}
TYPE: {briefing|entrega|revisão|alerta|aprovação}
CAMPAIGN: {nome da campanha}
STATUS: {pendente|em-andamento|concluído}
---

## Contexto
[Por que estou enviando essa mensagem]

## Solicitação / Entrega
[O que estou pedindo ou entregando]

## Prazo
[Quando preciso da resposta ou quando entrego]

## Referências
[Arquivos, tasks ou dados relevantes]

## Próximo Passo
→ {agente-próximo}: {ação esperada}
```

## Comandos de Comunicação (todos os agentes)

| Comando | Função |
|---------|--------|
| `*handoff-to {agente} {mensagem}` | Envia handoff para outro agente |
| `*check-inbox` | Lê mensagens pendentes no seu inbox |
| `*responder {agente}` | Responde handoff recebido |
| `*status-handoffs` | Ver todas as comunicações em aberto |

## Pipelines Pré-definidos

### Pipeline 1: Lançamento de Campanha
```
Vanguard → briefing → [Rex + Iris + Max em paralelo]
Rex → entrega copy → Iris (para integrar visual+texto)
Iris → entrega criativos → Max (para subir campanha)
Max → lança → Vera (para monitorar)
Vera → relatório → Vanguard (para decisão)
```

### Pipeline 2: Otimização Semanal
```
Vera → relatório de performance → Vanguard
Vanguard → decisão → Max (escalar/pausar) + Rex (nova copy) + Iris (novos criativos)
Max → executa → Vera (monitora)
```

### Pipeline 3: Urgência (campanha caindo)
```
Vera → alerta → Vanguard (prioridade alta)
Vanguard → diagnóstico → Max + Rex + Iris (simultâneo)
Todos → respostas → Vanguard → decisão em 24h
```
