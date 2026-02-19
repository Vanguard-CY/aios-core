# Ativar MCP e deixar o agente autônomo (sem pedir permissão toda hora)

## O que está configurado

No `.cursor/mcp.json` você tem:

| MCP | Função |
|-----|--------|
| **browser** | Navegar, clicar, preencher, screenshots. |
| **desktop-commander** | Automação de desktop. |

Foi adicionado **`autoApprove: true`** em cada servidor para o Cursor tentar não pedir permissão a cada uso (depende da versão do Cursor).

---

## Para eu ser autônomo (não pedir permissão toda vez)

### 1. Configuração no mcp.json (já feito)
- Os dois MCPs têm `"autoApprove": true`.
- **Reinicie o Cursor** para recarregar o `.cursor/mcp.json`.

### 2. Modo “Run Everything” no Cursor
- Abra **Configurações**: `Ctrl + ,`
- Procure por **“Auto Run”** ou **“Chat”**.
- Em **Chat → Auto Run** (ou equivalente):
  - Ative **“Run Everything”** (executar tudo sem aprovação), **ou**
  - Use **“Use Allow List”** e adicione os comandos/ferramentas que podem rodar sozinhos.

### 3. Allowlist de ferramentas MCP (se existir)
- Em **Configurações → MCP** ou **Tools & MCP**, veja se há **“Allowlist”** ou **“Auto-approve”** por servidor ou por ferramenta.
- Se houver, adicione o servidor **browser** (e **desktop-commander** se quiser) para não pedir permissão.

### 4. Na primeira vez que eu usar o MCP
- Se ainda aparecer **Allow / Deny**, clique em **“Allow”** ou **“Always allow”** / **“Sempre permitir”** (se o Cursor oferecer), para que as próximas chamadas sejam automáticas.

---

## Resumo

1. **Reiniciar o Cursor** (para carregar `autoApprove: true`).
2. **Configurações → Chat → Auto Run** → ativar **“Run Everything”** (ou preencher a allowlist).
3. Se ainda pedir permissão, na primeira vez escolher **Allow** / **Sempre permitir**.
4. Usar o **Composer** (Ctrl + I) para eu usar o browser/desktop de forma autônoma.

**Observação:** Em algumas versões do Cursor o `autoApprove` e o “Run Everything” não funcionam bem para MCP; nesses casos a allowlist ou “Sempre permitir” na primeira execução costuma ser o que resolve.
