# Git — Comandos úteis e difíceis de lembrar

---

## Branches e checkout

```bash
# Criar e já entrar numa branch nova
git checkout -b feature/minha-feature

# Trocar de branch (forma moderna)
git switch dev

# Ver todas as branches, incluindo remotas
git branch -a

# Deletar branch local
git branch -d minha-branch

# Deletar branch remota
git push origin --delete minha-branch

# Renomear a branch atual
git branch -m novo-nome
```

---

## Desfazer alterações

```bash
# Descartar mudança em um arquivo específico (o seu caso!)
# Útil quando um checkout trouxe sujeira sem querer
git checkout -- arquivo.txt

# Desfazer o último commit mantendo os arquivos no staging
git reset --soft HEAD~1

# Desfazer o último commit e jogar os arquivos de volta pro working dir
git reset --mixed HEAD~1

# Desfazer o último commit e APAGAR as mudanças (cuidado)
git reset --hard HEAD~1

# Desfazer um commit já publicado sem reescrever histórico
git revert abc1234
```

---

## Resetar branch ao estado remoto

```bash
# Útil quando checkouts acidentais trouxeram commits/alterações indesejadas
git fetch origin
git reset --hard origin/main
```

---

## Stash — guardar trabalho temporariamente

```bash
# Guardar com nome (muito mais fácil de achar depois)
git stash push -m "wip: ajuste no header"

# Guardar incluindo arquivos novos (untracked)
git stash push -u -m "wip: nova feature"

# Listar tudo que está guardado
git stash list

# Aplicar e remover o stash mais recente
git stash pop

# Aplicar um stash específico sem remover
git stash apply stash@{2}

# Deletar um stash específico
git stash drop stash@{0}

# Deletar todos os stashes
git stash clear
```

---

## Histórico e diagnóstico

```bash
# Log resumido e visual de branches
git log --oneline --graph --all

# Ver quem alterou cada linha de um arquivo
git blame -L 10,30 src/app.js

# Buscar uma string no histórico de commits
git log -S "nomeDaFuncao" --oneline

# Ver o que mudou num commit específico
git show abc1234

# Ver histórico de TUDO — checkouts, resets, rebases
# Se perdeu um commit, ele ainda está aqui
git reflog

# Voltar a um estado anterior via reflog
git checkout HEAD@{3}
```

---

## Cherry-pick — trazer só um commit

```bash
# Aplicar um commit específico na branch atual (ótimo para hotfixes)
git cherry-pick abc1234

# Cherry-pick sem commitar (fica no staging)
git cherry-pick abc1234 --no-commit

# Cherry-pick de um range de commits
git cherry-pick abc1234^..def5678
```

---

## Rebase interativo — editar histórico

```bash
# Editar, reordenar, fundir ou deletar os últimos N commits
git rebase -i HEAD~3
```

No editor que abrir, os comandos mais usados são:
- `pick` — mantém o commit como está
- `reword` — edita só a mensagem
- `squash` — funde com o commit anterior
- `drop` — deleta o commit

---

## Arquivos específicos

```bash
# Restaurar um arquivo de um commit antigo sem mexer no resto
git checkout abc1234 -- caminho/do/arquivo.js

# Adicionar só partes de um arquivo ao staging (modo interativo)
git add -p arquivo.js

# Remover um arquivo do staging sem perder a edição
git restore --staged arquivo.js

# Ver diferença entre staging e último commit
git diff --staged
```

---

## Limpeza

```bash
# Simular o que seria deletado (dry run — sempre fazer antes)
git clean -n

# Deletar arquivos não rastreados
git clean -f

# Deletar arquivos E pastas não rastreadas
git clean -fd

# Deletar também arquivos ignorados pelo .gitignore
git clean -fdx
```

---

## Bisect — encontrar qual commit introduziu um bug

```bash
git bisect start
git bisect bad                 # commit atual está com bug
git bisect good abc1234        # esse commit estava bom

# O git vai fazer checkout em commits intermediários.
# Para cada um, você testa e marca:
git bisect good
git bisect bad

# Quando encontrar o culpado, encerra:
git bisect reset
```

> Em um histórico de 1000 commits, encontra o culpado em ~10 passos.

---

## Tags

```bash
# Criar tag anotada (recomendada para releases)
git tag -a v1.0.0 -m "Release 1.0.0"

# Enviar tags pro remoto
git push origin --tags

# Deletar tag local e remota
git tag -d v1.0.0
git push origin --delete v1.0.0

# Ver todas as tags
git tag -l
```

---

## Remoto

```bash
# Ver os remotos configurados
git remote -v

# Adicionar um remoto
git remote add origin https://github.com/user/repo.git

# Mudar a URL de um remoto
git remote set-url origin https://github.com/user/novo-repo.git

# Baixar sem fazer merge (só atualiza o remoto local)
git fetch origin

# Puxar rebaseando em vez de merging (histórico mais limpo)
git pull --rebase origin main
```

---

## Aliases úteis pra colocar no ~/.gitconfig

```bash
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.st "status -sb"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.unstage "restore --staged"
```

Depois é só usar `git lg`, `git st`, `git undo`, etc.
