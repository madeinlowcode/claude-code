---
title: Como Instalar o MCP Obsidian no Claude Code
tags:
  - tutorial
  - mcp
  - obsidian
  - claude-code
date: '2026-03-02'
author: Script7
---
# Como Instalar o MCP Obsidian no Claude Code

> Tutorial completo para integrar seu vault do Obsidian com o Claude Code via MCP (Model Context Protocol).
> Baseado no repositório oficial: [bitbonsai/mcp-obsidian](https://github.com/bitbonsai/mcp-obsidian)

---

## Pré-requisitos

- **Node.js** v18.0.0 ou superior instalado ([nodejs.org](https://nodejs.org))
- **Claude Code** instalado globalmente (`npm i -g @anthropic-ai/claude-code`)
- Um **vault do Obsidian** (pasta local com arquivos `.md`)

---

## Passo 1 — Registrar o MCP Server

Abra o terminal e execute o comando abaixo, substituindo o caminho do vault pelo seu:

```bash
claude mcp add obsidian --scope user -- npx -y "@mauricio.wolff/mcp-obsidian@latest" "C:/caminho/para/seu/vault"
```

### Exemplos por sistema operacional

**Windows:**
```bash
claude mcp add obsidian --scope user -- npx -y "@mauricio.wolff/mcp-obsidian@latest" "C:/Users/SeuUsuario/Documents/MeuVault"
```

**macOS:**
```bash
claude mcp add obsidian --scope user -- npx -y "@mauricio.wolff/mcp-obsidian@latest" ~/Documents/MeuVault
```

**Linux:**
```bash
claude mcp add obsidian --scope user -- npx -y "@mauricio.wolff/mcp-obsidian@latest" ~/obsidian/MeuVault
```

> **Nota:** Se o caminho do vault contiver espaços, use aspas duplas ao redor do caminho.

---

## Passo 2 — Verificar a configuração

O comando anterior registra o servidor no arquivo `~/.claude.json`. Você pode verificar com:

```bash
claude mcp list
```

A configuração JSON gerada será semelhante a:

```json
{
  "mcpServers": {
    "obsidian": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@mauricio.wolff/mcp-obsidian@latest",
        "C:/caminho/para/seu/vault"
      ],
      "env": {}
    }
  }
}
```

---

## Passo 3 — Reiniciar o Claude Code

Feche e abra o Claude Code novamente para que o MCP seja carregado:

```bash
# Dentro do Claude Code, digite:
/exit

# Depois abra novamente:
claude
```

---

## Passo 4 — Testar a conexão

Dentro do Claude Code, peça para listar os arquivos do vault:

```
Liste os arquivos do meu vault do Obsidian
```

Se tudo estiver correto, o Claude usará a ferramenta `list_directory` e retornará os arquivos do seu vault.

---

## Ferramentas disponíveis (14 no total)

### Leitura
| Ferramenta | Descrição |
|---|---|
| `read_note` | Lê uma nota com frontmatter parseado |
| `read_multiple_notes` | Leitura em lote (máx. 10 arquivos) |
| `get_frontmatter` | Extrai apenas os metadados |
| `get_notes_info` | Metadados do arquivo sem conteúdo |
| `get_vault_stats` | Visão geral do vault e arquivos recentes |

### Escrita
| Ferramenta | Descrição |
|---|---|
| `write_note` | Cria/modifica nota (overwrite, append, prepend) |
| `patch_note` | Substitui texto específico dentro de uma nota |
| `update_frontmatter` | Modifica metadados sem alterar o conteúdo |

### Gerenciamento de arquivos
| Ferramenta | Descrição |
|---|---|
| `move_note` | Renomeia ou move arquivos markdown |
| `move_file` | Move qualquer tipo de arquivo |
| `delete_note` | Remove arquivo (requer confirmação) |

### Navegação e busca
| Ferramenta | Descrição |
|---|---|
| `list_directory` | Navega pela estrutura do vault |
| `search_notes` | Busca com reranking BM25 |
| `manage_tags` | Adiciona, remove e lista tags |

---

## Recursos de segurança

- Proteção contra path traversal (não permite escapar do vault)
- Exclusão automática de `.obsidian`, `.git` e `node_modules`
- Deleção exige confirmação com path duplicado
- Validação de frontmatter via gray-matter

---

## Troubleshooting

### "command not found: npx"
Instale o Node.js: [nodejs.org](https://nodejs.org)

### "Permission denied"
Verifique se a pasta do vault tem permissões de leitura e escrita.

### O Claude Code não reconhece o servidor
1. Confirme que o JSON em `~/.claude.json` está válido
2. Reinicie o Claude Code após qualquer alteração
3. Verifique se o caminho do vault está correto
4. Teste manualmente: `npx -y @mauricio.wolff/mcp-obsidian@latest "seu/vault"`

### Para debug avançado
```bash
npx @mauricio.wolff/mcp-obsidian /caminho/do/vault 2>debug.log
```

---

## Remover o MCP

Caso precise remover:

```bash
claude mcp remove obsidian --scope user
```

---

## Referências

- Repositório: [github.com/bitbonsai/mcp-obsidian](https://github.com/bitbonsai/mcp-obsidian)
- Pacote npm: [@mauricio.wolff/mcp-obsidian](https://www.npmjs.com/package/@mauricio.wolff/mcp-obsidian)
- Documentação MCP: [modelcontextprotocol.io](https://modelcontextprotocol.io)
