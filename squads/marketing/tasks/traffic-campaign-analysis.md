# Task: Análise de Campanhas Pagas

## Objetivo
Realizar análise completa de performance das campanhas ativas, identificar o que está funcionando, o que está drenando budget e gerar recomendações acionáveis.

## Inputs Necessários
- Plataforma(s) a analisar (Meta Ads / Google Ads / TikTok Ads)
- Período de análise
- Métricas disponíveis (ROAS, CPA, CTR, CPM, Conversões, Spend)
- Objetivo das campanhas (ROAS alvo, CPA máximo)

## Passo a Passo

### STEP 1: Coleta de Dados
Solicitar ao usuário que compartilhe os dados das campanhas:
- Export do Ads Manager ou screenshot/tabela com métricas
- ROAS alvo e CPA máximo definidos anteriormente
- Budget diário por campanha

### STEP 2: Análise por Camada
Analise em 3 camadas:

**Camada 1 — Campanha**
- Qual campanha tem melhor ROAS?
- Qual está abaixo do break-even?
- Budget está distribuído corretamente?

**Camada 2 — Conjunto de Anúncios**
- Qual público está convertendo mais?
- Qual tem frequência muito alta (>4)?
- Há sobreposição de públicos?

**Camada 3 — Anúncios**
- Qual criativo tem maior CTR?
- Qual tem menor CPA?
- Quais estão cansados (CTR caindo >50% do pico)?

### STEP 3: Classificação
Classifique cada campanha/conjunto em:
- 🟢 **ESCALAR** — ROAS acima do alvo, audiência sem saturar
- 🟡 **OTIMIZAR** — Performance mediana, há alavancas para melhorar
- 🔴 **PAUSAR/MATAR** — Abaixo do break-even há mais de 7 dias

### STEP 4: Identificação de Causa Raiz
Para cada item 🔴 ou 🟡, identifique:
- É problema de criativo? (CTR baixo = criativo fraco)
- É problema de copy? (CTR alto, conversão baixa = oferta/landing page)
- É problema de segmentação? (CPM alto, alcance pequeno)
- É problema de budget? (pouco dado para otimizar)

### STEP 5: Plano de Ação
Gerar lista priorizada de ações:
```
PRIORIDADE ALTA (fazer hoje):
1. [Ação específica] → [Campanha/Conjunto afetado]

PRIORIDADE MÉDIA (fazer esta semana):
1. [Ação específica] → [Campanha/Conjunto afetado]

PRÓXIMO TESTE:
1. [Hipótese] → [Como testar]
```

### STEP 6: Síntese Executiva
Resumo em formato de relatório para o @marketing-orchestrator:
- **Situação atual:** [1 frase]
- **O que está funcionando:** [bullet list]
- **Problemas identificados:** [bullet list]
- **Próximos passos:** [lista numerada]
- **Budget recomendado para próxima semana:** [distribuição]

## Output Esperado
Relatório de análise com classificação por campanha, causa raiz dos problemas e plano de ação priorizado.

## Colaboração
- Criativos fritos → briefar `@creative-director` via `*briefing-criativos`
- Copy com baixa conversão → briefar `@chief-copywriter` via `*copy-anuncio`
- Configuração errada → envolver `@media-buyer` via `*estruturar-campanha`
