# Requisitos — MCP do Blender

Para usar bem a skill **Mr.Blueprint**, você precisa ter o **MCP do Blender** instalado e conectado.

## Por quê

A skill trabalha em duas etapas:

1. **Blueprint:** desenha a blueprint técnica cotada. Essa etapa funciona sem MCP.
2. **Modelagem:** depois que você aprova a blueprint, ela abre o Blender, monta o modelo e exporta o STL.
   Essa etapa **precisa do MCP**.

Sem o MCP, a skill só entrega a blueprint e pede para você conectar o Blender antes de modelar.

## O que é necessário

- **Blender** instalado (versão 3.0 ou mais recente).
- **Addon MCP do Blender** instalado e ativo no Blender.
- **Servidor MCP do Blender** configurado no Claude (Claude Code, Claude Desktop ou claude.ai via dispositivo conectado).
- **Blender aberto e conectado** na hora de modelar.

## Como configurar (resumo)

1. Instale o addon no Blender: *Edit → Preferences → Add-ons → Install*, selecione o arquivo do addon e ative-o.
2. No Blender, abra a barra lateral (tecla `N`), vá na aba do MCP e clique em **Connect**.
3. Adicione o servidor MCP ao Claude. Exemplo no Claude Code:

   ```
   claude mcp add blender uvx blender-mcp
   ```

4. Confira a conexão: peça ao Claude para rodar `get_scene_info`. Se vier a lista da cena, está tudo pronto.

## Ferramentas usadas pela skill

- `get_scene_info`: verifica se o Blender está conectado e inspeciona a cena.
- `execute_blender_code`: roda o Python (bpy) que constrói o modelo.
- `look`: confere visualmente o resultado, se o seu MCP tiver essa ferramenta.

## Problemas comuns

| Sintoma | Solução |
|---|---|
| "Blender não conectado" | Abra o Blender e clique em **Connect** no painel do MCP. |
| Ferramentas do Blender não aparecem | Reinicie o Claude depois de adicionar o servidor MCP. |
| Erro de porta em uso | Feche outras instâncias do Blender ou do servidor MCP. |
