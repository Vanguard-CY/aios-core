# Task: Pipeline de Lançamento de Campanha

## Objetivo
Executar o pipeline completo de lançamento de campanha com comunicação automática entre os 3 times — do briefing estratégico até as campanhas no ar.

## Pré-condições
- Briefing aprovado pelo `@marketing-orchestrator`
- Budget definido
- Produto/oferta definidos

## Pipeline Completo

```
VANGUARD ──────────────────────────────────────────────────────
   │
   ├──[PARALELO]──────────────────────────────────────────────►
   │         │                    │                    │
  REX      IRIS                 MAX                 VERA
 (Copy)  (Design)            (Tráfego)           (Analytics)
   │         │                    │                    │
   │    Cria conceito        Define estrutura    Prepara dashboard
   │    visual               de campanha         de monitoramento
   │         │                    │                    │
   │◄────────┘                    │                    │
   │  Rex recebe conceito          │                    │
   │  visual e alinha copy         │                    │
   │         │                    │                    │
   │    Iris recebe copy           │                    │
   │    e integra ao visual        │                    │
   │         │                    │                    │
   │         └──────────────────►─┤                    │
   │              Max recebe       │                    │
   │              criativos+copy   │                    │
   │                               │                    │
   │                         BLADE (executa)            │
   │                         configura no Ads Manager   │
   │                               │                    │
   │                               └───────────────────►│
   │                                              Vera monitora
   │                                                    │
   └◄───────────────────────────────────────────────────┘
              Vanguard recebe relatório e decide
```

## Passo a Passo Detalhado

### FASE 1: Disparo (Vanguard → Todos) — Dia 0

**Vanguard executa:**
```
*handoff-to copy briefing
*handoff-to design briefing
*handoff-to traffic briefing
```

Handoff para Rex:
```
TYPE: briefing | PRIORITY: alta
Solicitar: copy completa (hooks + 3 versões + headlines)
Prazo: D+3
Entregar para: @creative-director após concluir
```

Handoff para Iris:
```
TYPE: briefing | PRIORITY: alta
Solicitar: conceito criativo + pacote de assets
Prazo: D+4 (aguarda copy do Rex)
Entregar para: @traffic-manager após concluir
```

Handoff para Max:
```
TYPE: briefing | PRIORITY: alta
Solicitar: estrutura de campanha pronta para subir
Aguardar: criativos da Iris antes de publicar
Prazo de publicação: D+5
```

---

### FASE 2: Produção Paralela — Dias 1-3

**Rex (Copy) executa:**
- `*avatar-cliente {nicho} {produto}`
- `*hook-textual {produto} {dor}`
- `*copy-anuncio {produto} {objetivo} {plataforma}`
- Ao concluir: `*handoff-to design entrega`

**Iris (Design) executa:**
- `*conceito-campanha {objetivo} {público}`
- Aguarda copy do Rex
- Integra texto + visual
- `*briefing-criativo` → `@visual-designer (Nova)` + `@video-editor (Flick)`

**Max (Tráfego) executa:**
- `*criar-campanha {objetivo} {budget}`
- `*estrategia-publico {produto} {nicho}`
- Prepara estrutura — aguarda assets

**Vera (Analytics) executa:**
- `*criar-dashboard {objetivo}`
- Define métricas de acompanhamento
- Prepara relatório semanal template

---

### FASE 3: Integração — Dias 3-4

**Rex → Iris:**
```
TYPE: entrega | PRIORITY: alta
Entregando: copy completa (hooks, 3 versões, headlines, CTAs)
Ação esperada: integrar ao conceito visual
```

**Iris → Max:**
```
TYPE: entrega | PRIORITY: alta
Entregando: pacote completo de criativos
Formatos: 2 vídeos 9:16 + 3 estáticos 1:1 + 1 carrossel
Ação esperada: subir campanhas conforme estrutura definida
```

---

### FASE 4: Lançamento — Dia 5

**Blade (Media Buyer) executa:**
- `*checklist-lancamento`
- Configura campanhas no Ads Manager
- Publica

**Max → Vanguard:**
```
TYPE: aprovação | PRIORITY: alta
Campanhas no ar: [lista]
Budget ativo: R$ X/dia
Primeiros dados disponíveis em: 48h
```

---

### FASE 5: Monitoramento — Dias 6-14

**Vera executa diariamente:**
- `*analisar-dados {período} {métricas}`

**Vera → Vanguard (relatório a cada 2 dias):**
```
TYPE: relatório | PRIORITY: média
CPA atual: R$ X
ROAS atual: X
Top criativo: [nome]
Recomendação: [escalar/otimizar/pausar]
```

**Vanguard → Max (quando necessário):**
```
TYPE: decisão
Ação: escalar conjunto B (ROAS 4.2x)
Ação: pausar conjunto A (CPA R$240, acima do limite)
```

---

### FASE 6: Otimização — Semana 2

**Max identifica vencedores:**
- `*escalar-campanha {id}`
- `*handoff-to copy novo-teste` (solicita novas variações)
- `*handoff-to design criativo-novo` (novos assets baseados no vencedor)

**Ciclo continua até fim do período ou decisão estratégica.**

---

## Output Final (Dia 28)
**Vera → Vanguard:** relatório executivo completo
**Vanguard:** decisão sobre próximo ciclo (escalar, pausar, pivotar)

## Tempo Total Estimado
- Fase 1 (Disparo): 30 min
- Fase 2-3 (Produção): 3-4 dias
- Fase 4 (Lançamento): 1 dia
- Fase 5-6 (Otimização): contínuo por 3 semanas
