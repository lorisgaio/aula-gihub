# 🐙 Guia Completo de Comandos Git & GitHub
> Do básico ao avançado — com explicações e casos de uso reais

---

## 📋 Índice

1. [Configuração Inicial](#1-configuração-inicial)
2. [Criando e Clonando Repositórios](#2-criando-e-clonando-repositórios)
3. [Trabalhando com Arquivos](#3-trabalhando-com-arquivos)
4. [Commits](#4-commits)
5. [Branches](#5-branches)
6. [Merge e Rebase](#6-merge-e-rebase)
7. [Repositórios Remotos](#7-repositórios-remotos)
8. [Inspeção e Histórico](#8-inspeção-e-histórico)
9. [Desfazendo Alterações](#9-desfazendo-alterações)
10. [Tags](#10-tags)
11. [Stash](#11-stash)
12. [Comandos Avançados](#12-comandos-avançados)
13. [GitHub CLI (gh)](#13-github-cli-gh)
14. [Cases de Uso Reais](#14-cases-de-uso-reais)
15. [Cheat Sheet Rápido](#15-cheat-sheet-rápido)

---

## 1. Configuração Inicial

Antes de qualquer coisa, configure sua identidade no Git. Essas informações aparecem em todos os seus commits.

```bash
git config --global user.name "Seu Nome"
```
> Define o nome que aparecerá nos seus commits.

```bash
git config --global user.email "seu@email.com"
```
> Define o e-mail vinculado aos seus commits (use o mesmo do GitHub).

```bash
git config --global core.editor "code --wait"
```
> Define o VS Code como editor padrão do Git. Troque por `vim`, `nano`, etc.

```bash
git config --global init.defaultBranch main
```
> Define `main` como nome padrão da branch principal (substitui o antigo `master`).

```bash
git config --list
```
> Lista todas as configurações ativas do Git.

```bash
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```
> Cria atalhos (aliases) para comandos longos. Após isso, `git st` = `git status`.

---

## 2. Criando e Clonando Repositórios

```bash
git init
```
> Inicializa um repositório Git na pasta atual. Cria a pasta oculta `.git`.

```bash
git init nome-do-projeto
```
> Cria uma nova pasta `nome-do-projeto` e inicializa um repositório dentro dela.

```bash
git clone https://github.com/usuario/repositorio.git
```
> Baixa um repositório remoto completo (com todo o histórico) para sua máquina.

```bash
git clone https://github.com/usuario/repositorio.git minha-pasta
```
> Clona o repositório dentro de uma pasta com nome personalizado.

```bash
git clone --depth 1 https://github.com/usuario/repositorio.git
```
> Clona apenas o último commit (clone raso). Útil para repositórios grandes quando você não precisa do histórico.

```bash
git clone --branch develop https://github.com/usuario/repositorio.git
```
> Clona o repositório já posicionado na branch `develop`.

---

## 3. Trabalhando com Arquivos

```bash
git status
```
> Mostra o estado atual do repositório: arquivos modificados, novos, deletados, staged ou não.

```bash
git add arquivo.py
```
> Adiciona um arquivo específico para a área de stage (preparando para commit).

```bash
git add .
```
> Adiciona **todos** os arquivos modificados/novos da pasta atual para o stage.

```bash
git add *.py
```
> Adiciona todos os arquivos `.py` para o stage.

```bash
git add -p
```
> Modo interativo: permite escolher partes específicas de um arquivo para adicionar ao stage (hunk by hunk). Muito útil para commits precisos.

```bash
git rm arquivo.py
```
> Remove o arquivo do repositório e do disco.

```bash
git rm --cached arquivo.py
```
> Remove o arquivo do rastreamento do Git, mas **mantém** o arquivo no disco. Útil para arquivos que foram adicionados por engano (ex: `.env`).

```bash
git mv arquivo_antigo.py arquivo_novo.py
```
> Renomeia ou move um arquivo e já registra a mudança no Git.

```bash
git restore arquivo.py
```
> Descarta as mudanças não staged de um arquivo, voltando à versão do último commit.

```bash
git restore --staged arquivo.py
```
> Remove o arquivo do stage sem perder as modificações.

```bash
.gitignore
```
> Arquivo de texto onde você lista padrões de arquivos/pastas que o Git deve ignorar.

**Exemplo de `.gitignore` para Python:**
```
__pycache__/
*.pyc
.env
venv/
.DS_Store
*.log
node_modules/
```

---

## 4. Commits

```bash
git commit -m "mensagem do commit"
```
> Salva as mudanças staged com uma mensagem descritiva.

```bash
git commit -am "mensagem"
```
> Adiciona **e** commita todos os arquivos já rastreados modificados em um só passo. Não inclui arquivos novos.

```bash
git commit --amend -m "nova mensagem"
```
> Altera o último commit: muda a mensagem ou adiciona arquivos esquecidos. **Não use em commits já enviados ao remoto.**

```bash
git commit --amend --no-edit
```
> Adiciona arquivos staged ao último commit sem alterar a mensagem.

### Boas práticas para mensagens de commit

Use o padrão **Conventional Commits**:

```
tipo(escopo): descrição curta

Corpo opcional explicando o porquê da mudança.
```

| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `docs` | Documentação |
| `style` | Formatação (sem mudança de lógica) |
| `refactor` | Refatoração de código |
| `test` | Adição/ajuste de testes |
| `chore` | Tarefas de manutenção, configs |

**Exemplos:**
```
feat(auth): adiciona login com Google OAuth
fix(api): corrige timeout na rota /usuarios
docs(readme): atualiza instruções de instalação
```

---

## 5. Branches

```bash
git branch
```
> Lista todas as branches locais. A branch atual aparece com `*`.

```bash
git branch -a
```
> Lista branches locais **e** remotas.

```bash
git branch nome-da-branch
```
> Cria uma nova branch (mas não muda para ela).

```bash
git checkout nome-da-branch
```
> Muda para a branch especificada.

```bash
git checkout -b nome-da-branch
```
> Cria e já muda para a nova branch. Atalho do `branch` + `checkout`.

```bash
git switch nome-da-branch
```
> Forma moderna de trocar de branch (substitui o `checkout` para esse uso).

```bash
git switch -c nova-branch
```
> Cria e muda para uma nova branch (equivalente ao `checkout -b`).

```bash
git branch -d nome-da-branch
```
> Deleta uma branch local (só funciona se já foi merged).

```bash
git branch -D nome-da-branch
```
> Força a deleção de uma branch local, mesmo sem merge.

```bash
git branch -m novo-nome
```
> Renomeia a branch atual.

```bash
git push origin --delete nome-da-branch
```
> Deleta uma branch no repositório remoto.

---

## 6. Merge e Rebase

### Merge

```bash
git merge nome-da-branch
```
> Integra as mudanças de outra branch na branch atual, criando um "merge commit".

```bash
git merge --no-ff nome-da-branch
```
> Força a criação de um merge commit mesmo quando seria possível um fast-forward. Mantém o histórico de que houve uma branch.

```bash
git merge --squash nome-da-branch
```
> Junta todos os commits da branch em um único conjunto de mudanças staged, sem criar o merge commit automaticamente. Você faz o commit manualmente depois.

```bash
git merge --abort
```
> Cancela um merge com conflito em andamento, voltando ao estado anterior.

### Rebase

```bash
git rebase main
```
> Reaplica os commits da branch atual sobre o topo da `main`, criando um histórico linear e limpo.

```bash
git rebase -i HEAD~3
```
> Rebase interativo dos últimos 3 commits. Permite reordenar, editar, juntar (squash) ou deletar commits.

**Opções do rebase interativo:**
```
pick   → mantém o commit
reword → mantém, mas edita a mensagem
edit   → para para editar o commit
squash → junta com o commit anterior
fixup  → igual ao squash, mas descarta a mensagem
drop   → remove o commit
```

```bash
git rebase --abort
```
> Cancela o rebase em andamento.

```bash
git rebase --continue
```
> Continua o rebase após resolver conflitos.

### Merge vs Rebase

| | Merge | Rebase |
|---|---|---|
| Histórico | Preserva com merge commits | Linear e limpo |
| Segurança | Seguro para branches públicas | **Evite em branches públicas** |
| Rastreabilidade | Fácil ver quando branches se uniram | Mais difícil |
| Uso ideal | Integração de features em main | Atualizar feature branch com main |

---

## 7. Repositórios Remotos

```bash
git remote -v
```
> Lista os repositórios remotos configurados com suas URLs.

```bash
git remote add origin https://github.com/usuario/repo.git
```
> Vincula seu repositório local ao remoto com o nome `origin`.

```bash
git remote rename origin upstream
```
> Renomeia um remoto.

```bash
git remote remove origin
```
> Remove a vinculação com o remoto.

```bash
git push origin main
```
> Envia os commits locais da branch `main` para o remoto `origin`.

```bash
git push -u origin main
```
> Envia e configura o tracking (rastreamento) da branch. Após isso, basta usar `git push`.

```bash
git push --force
```
> Força o envio sobrescrevendo o histórico remoto. **Perigoso em branches compartilhadas.**

```bash
git push --force-with-lease
```
> Versão mais segura do force push: só sobrescreve se ninguém mais tiver feito push desde seu último pull.

```bash
git push origin --tags
```
> Envia todas as tags locais para o remoto.

```bash
git fetch origin
```
> Baixa as atualizações do remoto **sem** aplicá-las na branch atual. Seguro para ver o que mudou.

```bash
git fetch --all
```
> Baixa atualizações de todos os remotos configurados.

```bash
git pull origin main
```
> Baixa (`fetch`) e aplica (`merge`) as mudanças do remoto na branch atual.

```bash
git pull --rebase origin main
```
> Baixa e aplica com rebase em vez de merge. Mantém histórico mais limpo.

---

## 8. Inspeção e Histórico

```bash
git log
```
> Exibe o histórico completo de commits com hash, autor, data e mensagem.

```bash
git log --oneline
```
> Histórico resumido: uma linha por commit.

```bash
git log --oneline --graph --all
```
> Histórico visual em árvore, mostrando todas as branches. Muito útil para entender o fluxo.

```bash
git log --author="Seu Nome"
```
> Filtra commits por autor.

```bash
git log --since="2024-01-01" --until="2024-12-31"
```
> Filtra commits por período.

```bash
git log -p arquivo.py
```
> Mostra o histórico de mudanças linha a linha de um arquivo específico.

```bash
git log --grep="fix"
```
> Filtra commits cuja mensagem contém "fix".

```bash
git show abc1234
```
> Mostra os detalhes e diff de um commit específico pelo hash.

```bash
git diff
```
> Mostra as diferenças entre o working directory e o stage (mudanças não staged).

```bash
git diff --staged
```
> Mostra as diferenças entre o stage e o último commit (o que será commitado).

```bash
git diff main feature-branch
```
> Compara duas branches.

```bash
git blame arquivo.py
```
> Mostra linha a linha quem fez cada alteração e em qual commit. Útil para rastrear quem introduziu um bug.

```bash
git shortlog -sn
```
> Resume os commits por autor, mostrando quantos cada um fez.

```bash
git bisect start
git bisect bad
git bisect good abc1234
```
> Busca binária no histórico para encontrar qual commit introduziu um bug.

---

## 9. Desfazendo Alterações

```bash
git restore arquivo.py
```
> Descarta mudanças não commitadas em um arquivo.

```bash
git restore .
```
> Descarta **todas** as mudanças não commitadas.

```bash
git reset HEAD~1
```
> Desfaz o último commit, mantendo as mudanças no working directory (soft-ish).

```bash
git reset --soft HEAD~1
```
> Desfaz o último commit, mantendo as mudanças **staged** (prontas para re-commitar).

```bash
git reset --mixed HEAD~1
```
> Desfaz o último commit, mantendo as mudanças no working directory mas **fora** do stage. (padrão)

```bash
git reset --hard HEAD~1
```
> Desfaz o último commit e **apaga** as mudanças. **Irreversível localmente.**

```bash
git revert abc1234
```
> Cria um novo commit que desfaz as mudanças de um commit anterior. **Seguro para histórico público** — não reescreve histórico.

```bash
git clean -fd
```
> Remove arquivos e pastas não rastreados pelo Git. `-f` força, `-d` inclui pastas.

```bash
git clean -n
```
> Simulação: mostra o que seria removido pelo `clean` sem remover de fato.

### Quando usar reset vs revert?

| Situação | Use |
|---|---|
| Commit só local, não enviado | `git reset` |
| Commit já enviado ao remoto | `git revert` |
| Precisa manter histórico | `git revert` |
| Branch pessoal, pode reescrever | `git reset` |

---

## 10. Tags

```bash
git tag
```
> Lista todas as tags.

```bash
git tag v1.0.0
```
> Cria uma tag leve (lightweight) no commit atual.

```bash
git tag -a v1.0.0 -m "Versão 1.0.0 - release inicial"
```
> Cria uma tag anotada com mensagem (recomendada para releases).

```bash
git tag -a v1.0.0 abc1234
```
> Cria uma tag em um commit específico pelo hash.

```bash
git show v1.0.0
```
> Exibe os detalhes de uma tag.

```bash
git push origin v1.0.0
```
> Envia uma tag específica para o remoto.

```bash
git push origin --tags
```
> Envia todas as tags para o remoto.

```bash
git tag -d v1.0.0
```
> Deleta uma tag local.

```bash
git push origin --delete v1.0.0
```
> Deleta uma tag no remoto.

---

## 11. Stash

O stash é uma "gaveta" temporária para guardar mudanças sem commitar.

```bash
git stash
```
> Salva as mudanças atuais (staged e não staged) e limpa o working directory.

```bash
git stash push -m "trabalhando no login"
```
> Salva o stash com uma mensagem descritiva.

```bash
git stash list
```
> Lista todos os stashes salvos.

```bash
git stash pop
```
> Aplica o stash mais recente e o remove da lista.

```bash
git stash apply stash@{2}
```
> Aplica um stash específico sem removê-lo da lista.

```bash
git stash drop stash@{0}
```
> Remove um stash específico.

```bash
git stash clear
```
> Remove todos os stashes.

```bash
git stash branch nova-branch
```
> Cria uma nova branch e aplica o stash nela. Útil quando o stash conflitaria com a branch atual.

---

## 12. Comandos Avançados

```bash
git cherry-pick abc1234
```
> Aplica um commit específico de qualquer branch na branch atual. Copia apenas aquele commit.

```bash
git cherry-pick abc1234..def5678
```
> Aplica um intervalo de commits.

```bash
git reflog
```
> Histórico de todos os movimentos do HEAD, incluindo resets e checkouts. **O salva-vidas do Git** — permite recuperar commits "perdidos".

```bash
git worktree add ../hotfix hotfix-branch
```
> Cria uma segunda cópia do repositório em outra pasta, ligada a outra branch. Permite trabalhar em duas branches simultaneamente.

```bash
git submodule add https://github.com/usuario/lib.git libs/lib
```
> Adiciona um repositório externo como submódulo dentro do projeto.

```bash
git submodule update --init --recursive
```
> Inicializa e atualiza todos os submódulos.

```bash
git archive --format=zip HEAD > projeto.zip
```
> Exporta o estado atual do repositório como arquivo ZIP, sem incluir o histórico Git.

```bash
git shortlog -sn --no-merges
```
> Ranking de contribuidores por número de commits (excluindo merges).

```bash
git ls-files
```
> Lista todos os arquivos rastreados pelo Git.

```bash
git count-objects -vH
```
> Mostra o tamanho do repositório Git.

```bash
git gc
```
> Garbage collect: limpa e otimiza o repositório.

---

## 13. GitHub CLI (gh)

O `gh` é a interface de linha de comando oficial do GitHub. Instale em [cli.github.com](https://cli.github.com).

```bash
gh auth login
```
> Autentica com sua conta GitHub.

```bash
gh repo create meu-projeto --public
```
> Cria um novo repositório no GitHub.

```bash
gh repo clone usuario/repositorio
```
> Clona um repositório do GitHub.

```bash
gh repo view --web
```
> Abre o repositório atual no navegador.

```bash
gh pr create --title "feat: nova feature" --body "Descrição da PR"
```
> Cria um Pull Request a partir da branch atual.

```bash
gh pr list
```
> Lista os Pull Requests abertos.

```bash
gh pr checkout 42
```
> Faz checkout de um PR pelo número.

```bash
gh pr merge 42 --squash
```
> Mergeia o PR #42 com squash.

```bash
gh issue create --title "Bug: erro no login" --body "Descrição do bug"
```
> Cria uma Issue no GitHub.

```bash
gh issue list --label "bug"
```
> Lista Issues filtradas por label.

```bash
gh workflow run deploy.yml
```
> Dispara manualmente um GitHub Actions workflow.

```bash
gh workflow list
```
> Lista os workflows do repositório.

```bash
gh run list
```
> Lista as execuções recentes de workflows.

```bash
gh release create v1.0.0 --notes "Release inicial"
```
> Cria um release no GitHub.

---

## 14. Cases de Uso Reais

---

### 🔥 Case 1: Fluxo diário de desenvolvimento (Feature Branch Workflow)

```bash
# 1. Atualiza a main
git checkout main
git pull origin main

# 2. Cria branch para a nova feature
git checkout -b feat/automacao-relatorio

# 3. Desenvolve e commita
git add scripts/relatorio.py
git commit -m "feat(relatorio): adiciona geração automática de PDF"

git add .
git commit -m "feat(relatorio): adiciona envio por e-mail"

# 4. Atualiza branch com as últimas mudanças da main
git fetch origin
git rebase origin/main

# 5. Envia para o remoto e abre PR
git push -u origin feat/automacao-relatorio
gh pr create --title "feat: automação de relatório" --body "Gera e envia relatório em PDF automaticamente"
```

---

### 🚑 Case 2: Hotfix em produção

```bash
# 1. Cria branch de hotfix a partir da main (ou tag de produção)
git checkout main
git pull origin main
git checkout -b hotfix/corrige-timeout-api

# 2. Corrige o bug
git add api/client.py
git commit -m "fix(api): aumenta timeout de 30s para 60s"

# 3. Mergeia em main E develop
git checkout main
git merge --no-ff hotfix/corrige-timeout-api
git tag -a v1.0.1 -m "Hotfix: corrige timeout"
git push origin main --tags

git checkout develop
git merge --no-ff hotfix/corrige-timeout-api
git push origin develop

# 4. Remove branch de hotfix
git branch -d hotfix/corrige-timeout-api
git push origin --delete hotfix/corrige-timeout-api
```

---

### 💾 Case 3: Salvando trabalho inacabado para trocar de contexto

```bash
# Você está no meio de uma feature quando chega uma tarefa urgente

# 1. Salva o trabalho atual
git stash push -m "WIP: integrando API do N8N"

# 2. Resolve a tarefa urgente em outra branch
git checkout main
git checkout -b fix/erro-webhook
# ... faz as alterações ...
git commit -am "fix: corrige parsing do webhook"
git push origin fix/erro-webhook

# 3. Volta para a feature e restaura o trabalho
git checkout feat/integracao-n8n
git stash pop
```

---

### 🔍 Case 4: Encontrando qual commit quebrou o código (git bisect)

```bash
# O código estava funcionando na v1.0, mas está quebrado agora

git bisect start
git bisect bad                    # commit atual está quebrado
git bisect good v1.0              # v1.0 estava funcionando

# Git vai fazer checkout em commits intermediários
# Para cada commit, você testa e informa:
git bisect good   # se esse commit funciona
git bisect bad    # se esse commit está quebrado

# Git encontra automaticamente o commit problemático
# Ao terminar:
git bisect reset
```

---

### ↩️ Case 5: Desfazendo um commit já enviado ao remoto

```bash
# Você fez push de um commit com dados sensíveis ou bug grave

# 1. Identifica o commit a ser revertido
git log --oneline

# 2. Reverte sem reescrever histórico (seguro)
git revert abc1234
git push origin main

# Se for um arquivo sensível (.env), também remova do histórico:
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch .env' \
  --prune-empty --tag-name-filter cat -- --all
git push origin --force --all
```

---

### 🍒 Case 6: Cherry-pick — aplicar um commit específico de outra branch

```bash
# Você tem um fix importante na branch develop
# e precisa dele na main sem mergear tudo

git log develop --oneline
# abc1234 fix: corrige cálculo de imposto

git checkout main
git cherry-pick abc1234
git push origin main
```

---

### 📦 Case 7: Automatizando com GitHub Actions (CI/CD para Python)

Crie o arquivo `.github/workflows/ci.yml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      
      - name: Run tests
        run: |
          python -m pytest tests/ -v
      
      - name: Check code style
        run: |
          pip install flake8
          flake8 . --max-line-length=88
```

---

### 🔐 Case 8: Removendo arquivo sensível do histórico

```bash
# Você acidentalmente commitou um arquivo .env com senhas

# Usando git-filter-repo (mais moderno que filter-branch)
pip install git-filter-repo

git filter-repo --path .env --invert-paths

# Adiciona ao .gitignore para não acontecer de novo
echo ".env" >> .gitignore
git add .gitignore
git commit -m "chore: adiciona .env ao gitignore"

# Força push (necessário após reescrever histórico)
git push origin --force --all
```

---

## 15. Cheat Sheet Rápido

```
──────────────────────────────────────────────────────────────────
SETUP
git config --global user.name "Nome"     → configura nome
git config --global user.email "email"   → configura e-mail

INÍCIO
git init                                 → novo repo local
git clone <url>                          → clonar repo remoto

CICLO DIÁRIO
git status                               → ver estado
git add .                                → stage tudo
git commit -m "mensagem"                 → commitar
git push                                 → enviar
git pull                                 → baixar e aplicar

BRANCHES
git switch -c feature/x                  → criar e mudar
git switch main                          → mudar de branch
git merge feature/x                      → mergear
git branch -d feature/x                  → deletar

DESFAZER
git restore arquivo                      → descartar mudanças
git reset --soft HEAD~1                  → desfaz commit (mantém staged)
git revert abc1234                       → reverte com segurança

INSPEÇÃO
git log --oneline --graph --all          → histórico visual
git diff                                 → ver mudanças
git blame arquivo                        → ver autoria

STASH
git stash                                → guardar temporariamente
git stash pop                            → restaurar

EMERGÊNCIAS
git reflog                               → recuperar qualquer coisa
git bisect                               → achar commit problemático
──────────────────────────────────────────────────────────────────
```

---

> **💡 Dica:** Para praticar Git de forma segura e interativa, acesse [learngitbranching.js.org](https://learngitbranching.js.org) — é gratuito e visual.

> **📚 Referência oficial:** [git-scm.com/docs](https://git-scm.com/docs)
