# Task: Criação de Nova Campanha

## Objetivo
Estruturar uma nova campanha paga do zero, com estratégia clara de objetivo, segmentação, budget e critérios de sucesso — pronta para ser implementada pelo `@media-buyer`.

## Inputs Necessários
- Objetivo da campanha (Conversão, Lead, Awareness, Tráfego)
- Produto/oferta a promover
- Budget disponível (diário ou total)
- Plataforma preferida (Meta Ads / Google Ads / TikTok)
- Prazo da campanha
- ROAS alvo ou CPA máximo

## Passo a Passo

### STEP 1: Diagnóstico Estratégico
Antes de criar, validar:
- **O produto tem proof?** — há resultados para usar como prova social?
- **A oferta está clara?** — o que o cliente recebe? por quanto? por que agora?
- **O funil está pronto?** — landing page, checkout, pixel configurado?

Se qualquer resposta for "não" → resolver antes de investir em tráfego.

### STEP 2: Definir Estratégia de Campanha

**Objective mapping:**
```
Objetivo = Venda direta       → Campanha de Conversão
Objetivo = Leads qualificados → Campanha de Geração de Cadastros
Objetivo = Lançamento         → Sequência TOF Awareness → MOF Conversão
Objetivo = Escalar algo pronto → Duplicar campanha vencedora + expandir público
```

**Budget allocation:**
```
Budget total → dividir por:
  - 60% Campanhas de conversão (BOF)
  - 30% Geração de leads / MOF
  - 10% Awareness / TOF
```

### STEP 3: Estrutura de Campanha
Definir a arquitetura completa:

```
CAMPANHA: [Nome seguindo naming convention]
  Objetivo: [Conversão/Leads/etc]
  Budget: R$ [X]/dia
  
  CONJUNTO 1: [Público Frio — Interesse]
    Público: [Interesses específicos]
    Faixa etária: [X-X]
    Gênero: [M/F/Todos]
    Localização: [Cidades/Regiões]
    Budget: R$ [X]/dia
    
  CONJUNTO 2: [Lookalike — Compradores]
    Público: LAL 1% compradores últimos 180d
    Budget: R$ [X]/dia
    
  CONJUNTO 3: [Retargeting — Visitantes]
    Público: Visitaram LP últimos 30d, NÃO compraram
    Budget: R$ [X]/dia
```

### STEP 4: Definir Criativos Necessários
Listar os criativos que a campanha precisa para testar:
- Quantos criativos por conjunto
- Formatos necessários (1:1, 9:16, carrossel)
- Tipo de criativo (estático, vídeo, UGC)

→ Gerar briefing para `@creative-director` via `*briefing-criativo`

### STEP 5: Definir Copy Necessária
Listar as copies que precisam ser produzidas:
- Primário (texto do anúncio)
- Headline
- Descrição
- CTA

→ Gerar briefing para `@chief-copywriter` via `*copy-anuncio`

### STEP 6: Critérios de Sucesso e Decisão

Definir claramente:
```
✅ Escalar se: ROAS > [X] por 3 dias consecutivos
⚡ Otimizar se: ROAS entre [X] e [Y] por 5 dias
❌ Pausar se: ROAS < [X] por 7 dias sem melhora
⏸️ Não julgar antes de: [N] conversões ou [N] dias
```

### STEP 7: Checklist de Pré-Lançamento
Passar para o `@media-buyer` com:
- Estrutura de campanha definida
- Lista de públicos a criar
- Pixel de conversão a verificar
- Naming convention a seguir

## Output Esperado
Documento de campanha completo com: estrutura, públicos, budget, criativos necessários, copy necessária, critérios de sucesso e próximos passos para o `@media-buyer` executar.
