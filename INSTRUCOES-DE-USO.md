# Instruções de uso — Mr.Blueprint

O pacote tem duas skills:

| Comando | O que faz |
|---|---|
| `/mr-blueprint-configuracao` | Instala e configura o MCP do Blender no seu PC, passo a passo. Rode **uma vez**, antes de tudo. |
| `/mr-blueprint` | Cria a blueprint técnica cotada, pede sua aprovação e modela a peça no Blender. |

---

## 1. Primeira vez: configurar o MCP do Blender

1. No Claude Code, digite:
   ```
   /mr-blueprint-configuracao
   ```
2. O Claude faz um **diagnóstico** e mostra o que falta:
   ```
   [ ] Blender   [ ] uv   [ ] Addon MCP   [ ] Servidor MCP no Claude   [ ] Conexão
   ```
3. Ele segue as etapas uma por vez. Em cada uma, **explica o que vai fazer e pede sua permissão** antes de mexer no PC:
   - **Blender:** procura no PC; se não achar, instala pelo `winget`.
   - **uv:** instala o `uv`, que roda o servidor `blender-mcp`.
   - **Addon:** baixa o addon oficial (`ahujasid/blender-mcp`) e ativa no Blender. Feche o Blender quando ele pedir.
   - **Servidor MCP:** registra o servidor `blender` no Claude Code (ou no `claude_desktop_config.json` do Claude Desktop, com backup).
4. No final, você faz duas coisas à mão:
   - Abre o Blender → aperta **N** → aba **BlenderMCP** → **Connect to MCP server**.
   - Reinicia o Claude.
5. O Claude testa a conexão com `get_scene_info`. Se der certo, aparece **"Pronto!"**.

> Sem terminal (ex.: claude.ai)? A skill passa as mesmas etapas como instruções manuais. Responda "feito" a cada uma.

## 2. Criar uma peça

1. Abra o Blender e clique em **Connect** na aba BlenderMCP.
2. No Claude, descreva a peça:
   ```
   /mr-blueprint suporte de parede para fone, 80 mm de largura, para impressão 3D
   ```
3. O Mr.Blueprint entrega:
   - a blueprint (vistas frontal, lateral, superior e isométrica, com cotas em mm);
   - a tabela de medidas, o plano de modelagem e as suposições.
4. Responda **"aprovado"** para modelar, ou diga o que mudar (ex.: "aumenta a parede para 3 mm").
5. Depois da aprovação, ele modela no Blender, confere as dimensões e exporta **STL** e **.blend**.

## Dicas

- Informe tamanho, função e se é para impressão 3D. Assim a blueprint sai certa na primeira vez.
- Use o MCP do Blender em **um cliente por vez** (Claude Code *ou* Claude Desktop).
- A skill só faz designs originais, objetos genéricos ou peças suas. Não faz personagens, logos ou produtos de marca.

## Problemas?

Rode `/mr-blueprint-configuracao` de novo: ele refaz o diagnóstico e corrige só o que estiver faltando. Veja também `REQUISITOS.md`.
