# Guia de Instalação - Lisa Plugin

## 📚 Sobre o Lisa

O **Lisa** é um plugin para Claude Code que implementa loops iterativos observáveis. É uma evolução do "Ralph Wiggum technique" que permite executar tarefas complexas de forma iterativa com segurança e observabilidade.

**Repositório Original:** https://github.com/Arakiss/lisa

---

## 🚀 Requisitos

- Claude Code instalado
- Permissões para instalar plugins
- Git (para instalação local)

---

## 📦 Opção 1: Instalação via Marketplace (Recomendado)

Esta é a forma mais simples e recomendada para a maioria dos usuários.

### Passo 1: Adicionar o Marketplace

No Claude Code, execute:
```bash
/plugin marketplace add Arakiss/lisa
```

### Passo 2: Instalar o Plugin
```bash
/plugin install lisa@lisa-marketplace
```

### Usando CLI (Alternativa)

Se preferir usar a linha de comando:
```bash
claude plugin marketplace add Arakiss/lisa
claude plugin install lisa@lisa-marketplace
```

### ✅ Verificar Instalação

Execute o comando para confirmar que tudo está funcionando:
```bash
/lisa:help
```

Se a ajuda for exibida, a instalação foi bem-sucedida! 🎉

---

## 🔧 Opção 2: Instalação Local (Manual)

Use esta opção se preferir ter o código localmente ou contribuir para o projeto.

### Passo 1: Clonar o Repositório
```bash
mkdir -p ~/.claude/plugins/local/plugins
git clone git@github.com:Arakiss/lisa.git ~/.claude/plugins/local/plugins/lisa
```

> **Nota:** Se não tiver SSH configurado, use HTTPS:
> ```bash
> git clone https://github.com/Arakiss/lisa.git ~/.claude/plugins/local/plugins/lisa
> ```

### Passo 2: Criar/Atualizar plugins.json
```bash
echo '{"plugins":["lisa"]}' > ~/.claude/plugins/local/plugins.json
```

### Passo 3: Configurar ~/.claude/settings.json

Abra o arquivo `~/.claude/settings.json` e adicione à seção `enabledPlugins`:
```json
{
  "enabledPlugins": {
    "lisa@local-plugins": true
  }
}
```

### Passo 4: Configurar ~/.claude/plugins/installed_plugins.json

Abra ou crie o arquivo `~/.claude/plugins/installed_plugins.json` e adicione na seção `plugins`:
```json
{
  "plugins": [
    {
      "scope": "user",
      "installPath": "/YOUR/HOME/.claude/plugins/local/plugins/lisa",
      "version": "1.3.0",
      "installedAt": "2026-01-06T00:00:00.000Z",
      "lastUpdated": "2026-01-06T00:00:00.000Z",
      "isLocal": true
    }
  ]
}
```

> ⚠️ **IMPORTANTE:** Substitua `/YOUR/HOME/` pelo caminho completo do seu diretório home:
> - **Linux/Mac:** `/Users/seu-usuario` ou `/home/seu-usuario`
> - **Windows:** `C:\\Users\\seu-usuario`

### Passo 5: Reiniciar Claude Code

Feche e reabra o Claude Code para que as mudanças entrem em efeito.

### ✅ Verificar Instalação
```bash
/lisa:help
```

---

## 🔄 Atualizando o Plugin

### Atualizar via Marketplace

Se instalou pela marketplace:
```bash
/plugin update lisa@lisa-marketplace
```

### Atualizar Instalação Local

Se instalou localmente:
```bash
cd ~/.claude/plugins/local/plugins/lisa
git pull
```

---

## 📋 Primeiros Passos após Instalação

### 1. Explorar os Comandos Disponíveis
```bash
/lisa:help
```

### 2. Criar um Prompt Simples

Crie um arquivo `PROMPT.md`:
```markdown
# Mission
Criar um script Hello World em Python.

# Requirements
- Arquivo main.py
- Script que imprime "Hello, World!"
- Executável

# Completion
Quando o script está pronto e funciona: <promise>DONE</promise>
```

### 3. Executar o Loop
```bash
/lisa:loop PROMPT.md --max-iterations 10
```

### 4. Verificar Status (enquanto rodando)
```bash
/lisa:status
```

### 5. Limpar Arquivos Órfãos
```bash
/lisa:clean --all
```

---

## 🎯 Comandos Principais

| Comando | Descrição |
|---------|-----------|
| `/lisa:loop <prompt>` | Inicia um loop iterativo |
| `/lisa:status` | Verifica status do loop ativo |
| `/lisa:cancel` | Cancela o loop atual |
| `/lisa:clean` | Remove arquivos órfãos e logs |
| `/lisa:prep` | Cria scaffolding para novo loop |
| `/lisa:help` | Mostra ajuda e documentação |

---

## ⚙️ Opções do /lisa:loop
```bash
/lisa:loop <prompt> [options]
```

### Opções Disponíveis

- `--max-iterations <n>` - Limite máximo de iterações (obrigatório para tarefas complexas)
- `--completion-promise <text>` - Sobrescreve a promessa auto-detectada
- `--stop-command <cmd>` - Comando externo para verificar condição de parada
- `--stop-when <value>` - Valor esperado do stop-command para parar

### Exemplos
```bash
# Simples com arquivo
/lisa:loop PROMPT.md --max-iterations 50

# Com promessa customizada
/lisa:loop PROMPT.md --max-iterations 30 --completion-promise "FINISHED"

# Com condição dinâmica (parar quando não há erros TS)
/lisa:loop "Fix TS errors" --stop-command "tsc --noEmit 2>&1 | grep -c error || echo 0" --stop-when "0"
```

---

## 🐛 Solução de Problemas

### Problema: Loop Roda Infinitamente

**Causa:** Sem tag `<promise>` ou `--max-iterations` não definido

**Solução:**
```bash
# Verificar se existe promise
grep "<promise>" PROMPT.md

# Ou adicionar max-iterations
/lisa:loop PROMPT.md --max-iterations 50
```

### Problema: Loop Encerra Muito Cedo

**Causa:** Promessa é acionada antes do trabalho estar completo

**Solução:** Torne a promessa mais específica:
```markdown
# ❌ Ruim
<promise>DONE</promise>

# ✅ Bom
<promise>ALL 48 CHAPTERS WRITTEN AND VALIDATED</promise>
```

### Problema: Arquivos Órfãos Acumulando

**Solução:**
```bash
/lisa:clean --all
```

### Problema: Não Consigo Ver o Que Está Acontecendo

**Solução:** Verifique o log:
```bash
tail -f .claude/lisa-loop.log
```

---

## 📚 Recursos Adicionais

- **Repositório Oficial:** https://github.com/Arakiss/lisa
- **Arquivo CHANGELOG:** https://github.com/Arakiss/lisa/blob/main/CHANGELOG.md
- **Guia de Contribuição:** https://github.com/Arakiss/lisa/blob/main/CONTRIBUTING.md
- **Licença:** MIT (https://github.com/Arakiss/lisa/blob/main/LICENSE)

---

## 💡 Dicas Úteis

1. **Use arquivos PROMPT.md** - Mais fácil de iterar e organizar
2. **Sempre defina --max-iterations** - Proteção contra loops infinitos
3. **Crie IMPLEMENTATION_PLAN.md** - Para rastrear progresso automaticamente
4. **Use spec-based verification** - Para tarefas críticas com múltiplos requisitos
5. **Verifique logs regularmente** - `tail -f .claude/lisa-loop.log` é seu amigo

---

## ✨ Recursos Avançados

### Rastreamento de Progresso

Se você tem um arquivo `IMPLEMENTATION_PLAN.md` com checkboxes:
```markdown
## Tasks
- [x] Setup project structure
- [x] Create database schema
- [ ] Implement API endpoints
- [ ] Write tests
```

Lisa automaticamente mostra progresso:
```
🔄 Lisa iteration 15 | Progress: 2/4 | To stop: <promise>DONE</promise>
```

### Verificação Baseada em Specs

Para tarefas críticas, crie `specs/features.md` com requisitos verificáveis:
```markdown
## AUTH-1: User Registration
- [ ] POST /auth/register accepts { email, password, name }
- [ ] Password minimum 8 chars, 1 uppercase, 1 number
- [ ] Returns 201 with { user, token }
- [ ] Returns 409 if email exists
```

Seu prompt deve ler e verificar este arquivo antes de prometer conclusão.

---

## 📞 Suporte

Se encontrar problemas:

1. Verifique o arquivo de log: `.claude/lisa-loop.log`
2. Consulte a documentação oficial: https://github.com/Arakiss/lisa
3. Abra uma issue no repositório: https://github.com/Arakiss/lisa/issues

---

**Última atualização:** Janeiro 2026
**Versão Lisa:** 1.5.0
