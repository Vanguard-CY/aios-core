# Task: Handoff Entre Agentes

## Objetivo
Padronizar a forma como um agente envia e recebe mensagens de outros agentes do squad de marketing, garantindo continuidade de trabalho sem perda de contexto.

## Comandos

### *handoff-to {agente} {tipo}
Envia uma mensagem/entrega estruturada para outro agente.

**Passo a Passo:**

1. Identificar o agente de destino e seu inbox:
   ```
   @traffic-manager  → squads/marketing/handoffs/traffic/inbox.md
   @creative-director→ squads/marketing/handoffs/design/inbox.md
   @chief-copywriter → squads/marketing/handoffs/copy/inbox.md
   @marketing-orchestrator → squads/marketing/handoffs/orchestrator/inbox.md
   ```

2. Escrever o handoff no formato padrão:
   ```markdown
   ---
   FROM: {seu-id}
   TO: {agente-destino}
   DATE: {data-atual}
   PRIORITY: {alta|média|baixa}
   TYPE: {briefing|entrega|revisão|alerta|aprovação}
   CAMPAIGN: {nome}
   STATUS: pendente
   ---
   
   ## Contexto
   [Situação que motivou este handoff]
   
   ## Solicitação / Entrega
   [O que você está pedindo OU entregando com todos os detalhes]
   
   ## Prazo
   [Quando precisa]
   
   ## Referências
   [Arquivos ou dados relevantes]
   
   ## Próximo Passo
   → @{destinatário}: {ação esperada}
   ```

3. Registrar no log:
   ```
   squads/marketing/handoffs/log/communication-log.md
   Adicionar linha: [DATA] FROM:{remetente} TO:{destinatário} TYPE:{tipo} CAMPAIGN:{campanha}
   ```

4. Notificar o Orchestrator se a prioridade for ALTA

---

### *check-inbox
Lê o inbox do agente atual e lista mensagens pendentes.

**Passo a Passo:**

1. Abrir o arquivo inbox do agente atual
2. Listar todas as mensagens com STATUS: pendente
3. Para cada mensagem pendente, mostrar:
   - Quem enviou
   - Tipo (briefing/entrega/etc)
   - Prioridade
   - Prazo
4. Perguntar: "Qual deseja processar primeiro?"

---

### *responder {agente}
Responde a um handoff recebido, seja com uma entrega ou com uma dúvida.

**Passo a Passo:**

1. Marcar a mensagem processada como STATUS: em-andamento
2. Executar a tarefa solicitada
3. Escrever handoff de resposta com:
   - TYPE: entrega (se concluiu) ou TYPE: revisão (se precisa de clareza)
   - Toda a entrega ou dúvida no corpo
4. Marcar o handoff original como STATUS: concluído
5. Registrar no log

---

### *status-handoffs
Visão geral de todas as comunicações em aberto entre os agentes.

**Output esperado:**
```
📬 HANDOFFS EM ABERTO
━━━━━━━━━━━━━━━━━━━━
🔴 ALTA PRIORIDADE:
  [Data] @vera → @marketing-orchestrator: alerta performance | PENDENTE

🟡 MÉDIA PRIORIDADE:
  [Data] @marketing-orchestrator → @rex: briefing copy | EM-ANDAMENTO
  [Data] @iris → @max: criativos prontos | PENDENTE

🟢 BAIXA PRIORIDADE:
  [Data] @sage → @flow: pauta email semanal | PENDENTE

✅ CONCLUÍDOS HOJE:
  [Data] @rex → @iris: copy entregue
```
