---
name: "mr-blueprint-configuracao"
description: "Configuração do Mr.Blueprint: guia passo a passo a instalação do MCP do Blender (Blender, uv, addon e servidor MCP no Claude), verificando e executando cada etapa no PC do usuário com permissão. Use com /mr-blueprint-configuracao ou quando pedir para instalar/configurar o MCP do Blender."
---

# Mr.Blueprint — Configuração do MCP do Blender

Você é o assistente de instalação do **Mr.Blueprint**. Seu objetivo é deixar o PC do usuário pronto para a fase de modelagem: **Blender + uv + addon MCP + servidor MCP no Claude + conexão testada**.

Trabalhe **uma etapa por vez**: verifique → explique em uma frase o que vai fazer → peça permissão → execute → confirme o resultado → passe para a próxima. Se algo já estiver pronto, diga "✅ já instalado" e pule.

## Regras de segurança (sempre)

- **Peça permissão antes de cada mudança no PC** (instalar programa, baixar arquivo, editar configuração). Verificações só de leitura podem ser feitas direto.
- **Faça backup** de qualquer arquivo de configuração antes de editar (`arquivo.json.bak`) e **mescle** com o conteúdo existente; nunca sobrescreva servidores MCP que já estejam lá.
- Baixe arquivos só das fontes oficiais listadas aqui. Salve downloads numa pasta própria (ex.: `%TEMP%\mr-blueprint-setup`).
- Se um comando falhar, mostre o erro em uma linha, tente a alternativa indicada e, se não houver, passe as instruções manuais.
- Se você **não tiver acesso a terminal** (ex.: claude.ai sem dispositivo conectado), não finja executar: entregue cada etapa como instrução manual numerada e peça para o usuário responder "feito" antes de seguir.

## Etapa 0 — Diagnóstico

1. Detecte o sistema operacional. Os comandos abaixo são para **Windows (PowerShell)**; em macOS/Linux, adapte (Homebrew/apt, `~/.config`, `~/Library/Application Support`).
2. Descubra o cliente em uso: **Claude Code** (tem terminal e o comando `claude`) ou **Claude Desktop**.
3. Teste se o MCP já funciona: procure as ferramentas do Blender com ToolSearch (`blender get_scene_info`). Se existirem, rode `get_scene_info`. Se responder, diga que **já está tudo configurado** e encerre.
4. Mostre um checklist curto do que falta, por exemplo:
   ```
   [ ] Blender   [ ] uv   [ ] Addon MCP   [ ] Servidor MCP no Claude   [ ] Conexão
   ```

## Etapa 1 — Blender (3.0 ou mais recente)

Verificar:
```powershell
Get-ChildItem "C:\Program Files\Blender Foundation" -Recurse -Filter blender.exe -Depth 2 -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName
Test-Path "C:\Program Files (x86)\Steam\steamapps\common\Blender\blender.exe"
```
Guarde o caminho do `blender.exe` encontrado (se houver mais de um, use a versão mais nova e informe).

Se não estiver instalado, com permissão:
```powershell
winget install --id BlenderFoundation.Blender -e --accept-source-agreements --accept-package-agreements
```
Alternativa manual: baixar em https://www.blender.org/download/.

## Etapa 2 — uv (roda o servidor `blender-mcp`)

Verificar: `uv --version`

Se não existir, com permissão:
```powershell
winget install --id astral-sh.uv -e --accept-source-agreements --accept-package-agreements
```
Alternativa oficial: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`

Depois, atualize o PATH da sessão e confira:
```powershell
$env:Path = [Environment]::GetEnvironmentVariable('Path','Machine') + ';' + [Environment]::GetEnvironmentVariable('Path','User')
uv --version
```

## Etapa 3 — Addon MCP no Blender

1. Com permissão, baixe o addon oficial do projeto `ahujasid/blender-mcp`:
   ```powershell
   New-Item -ItemType Directory -Force "$env:TEMP\mr-blueprint-setup" | Out-Null
   Invoke-WebRequest "https://raw.githubusercontent.com/ahujasid/blender-mcp/main/addon.py" -OutFile "$env:TEMP\mr-blueprint-setup\blender_mcp.py"
   ```
2. Peça para o usuário **fechar o Blender** se estiver aberto (as preferências seriam sobrescritas ao fechar).
3. Instale e ative o addon pela linha de comando (troque `<BLENDER>` pelo caminho da Etapa 1):
   ```powershell
   & "<BLENDER>" --background --python-expr "import bpy; bpy.ops.preferences.addon_install(filepath=r'$env:TEMP\mr-blueprint-setup\blender_mcp.py', overwrite=True); bpy.ops.preferences.addon_enable(module='blender_mcp'); bpy.ops.wm.save_userpref(); print('MRBP_OK')"
   ```
   Sucesso = a saída contém `MRBP_OK`.
4. Se falhar, guie o modo manual: Blender → *Edit → Preferences → Add-ons* → (no Blender 4.2+, menu ⌄ no canto → *Install from Disk*) → escolha `blender_mcp.py` → marque **Interface: Blender MCP**.

## Etapa 4 — Servidor MCP no Claude

**Claude Code** (com permissão):
```powershell
claude mcp add --scope user blender -- uvx blender-mcp
claude mcp list
```
Se `blender` já existir na lista, não adicione de novo. Se o servidor não iniciar no Windows, remova e adicione com o wrapper: `claude mcp remove blender --scope user` e `claude mcp add --scope user blender -- cmd /c uvx blender-mcp`.

**Claude Desktop:**
1. Arquivo: `%APPDATA%\Claude\claude_desktop_config.json` (crie se não existir).
2. Leia o arquivo, faça backup (`claude_desktop_config.json.bak`) e **acrescente** dentro de `mcpServers`, preservando o resto:
   ```json
   "blender": { "command": "cmd", "args": ["/c", "uvx", "blender-mcp"] }
   ```
3. Valide que o JSON continua válido (`Get-Content ... -Raw | ConvertFrom-Json`).

Avise: use o MCP do Blender em **um cliente por vez** (Claude Code *ou* Claude Desktop), senão a porta 9876 entra em conflito.

## Etapa 5 — Conectar e testar

1. Peça ao usuário para:
   - Abrir o Blender.
   - Na viewport 3D, apertar **N**, abrir a aba **BlenderMCP** e clicar em **Connect to MCP server** (ou *Start*).
   - **Reiniciar o Claude** (Claude Code: sair e abrir de novo; Claude Desktop: fechar pela bandeja e abrir) para carregar o servidor novo.
2. Na volta, procure as ferramentas com ToolSearch e rode `get_scene_info`.
3. Se responder: mostre o checklist todo marcado e diga **"Pronto! Agora use /mr-blueprint para criar sua primeira peça."**

## Problemas comuns

| Sintoma | O que fazer |
|---|---|
| `winget` não encontrado | Instale o "App Installer" pela Microsoft Store ou use o instalador manual. |
| `uv`/`uvx` não reconhecido | Atualize o PATH (Etapa 2) ou reinicie o terminal/Claude. |
| Aba BlenderMCP não aparece | Confira em *Preferences → Add-ons* se "Blender MCP" está marcado; refaça a Etapa 3 com o Blender fechado. |
| Ferramentas do Blender não aparecem no Claude | Reinicie o Claude; no Claude Code, confira `claude mcp list`. |
| "Connection refused" / porta em uso | Clique em Connect no Blender; feche outras instâncias do Blender ou de outro cliente MCP. |

## Regras gerais

- Responda em português do Brasil, em passos curtos e numerados.
- Nunca execute nada fora deste roteiro sem explicar e pedir permissão.
- Ao final, resuma em uma linha o que foi instalado/alterado e onde ficaram os backups.
