# Mr.Blueprint — skill

Cria uma blueprint técnica cotada (SVG) para um modelo 3D, pede aprovação e, se aprovada, modela no Blender.

## Conteúdo

- `mr-blueprint/SKILL.md` — a skill principal (`/mr-blueprint`)
- `mr-blueprint-configuracao/SKILL.md` — instalador guiado do MCP do Blender (`/mr-blueprint-configuracao`)
- `mr-blueprint.zip` e `mr-blueprint-configuracao.zip` — pacotes prontos para upload
- `INSTRUCOES-DE-USO.md` — passo a passo de uso
- `REQUISITOS.md` — o que o MCP do Blender precisa

## Como instalar

**Claude Code (local):** copie as pastas `mr-blueprint` e `mr-blueprint-configuracao` para `%USERPROFILE%\.claude\skills\`
(ou para `.claude\skills\` dentro de um projeto).

**claude.ai:** Configurações → Capabilities → Skills → Upload skill → envie `mr-blueprint.zip` e `mr-blueprint-configuracao.zip`.

## Primeiros passos

1. Rode **`/mr-blueprint-configuracao`**. Ele verifica o PC e instala, com sua permissão, o que faltar:
   Blender, `uv`, addon MCP e o servidor `blender` no Claude. No fim, testa a conexão.
2. Abra o Blender, aperte **N** → aba **BlenderMCP** → **Connect**.
3. Use **`/mr-blueprint`** e descreva a peça.

Detalhes em `INSTRUCOES-DE-USO.md`.

## Requisitos

- Para a fase de modelagem: Blender aberto com o addon MCP ativo. Veja `REQUISITOS.md`
  (ou rode `/mr-blueprint-configuracao` para instalar tudo automaticamente).
