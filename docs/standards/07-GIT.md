# 07 - GIT BÁSICO

## Branches Simples

Usar apenas **3 branches principais**:

```
main        → Código em produção (estável)
develop     → Código em desenvolvimento
feature/*   → Novas funcionalidades
```

---

## Workflow Básico

### 1. Começar uma nova feature
```bash
# Baixar atualizações
git pull origin develop

# Criar branch da feature
git checkout -b feature/nome-da-feature
```

### 2. Trabalhar na feature
```bash
# Ver mudanças
git status

# Adicionar arquivos
git add .

# Fazer commit
git commit -m "feat: descrição da mudança"

# Enviar para repositório
git push origin feature/nome-da-feature
```

### 3. Terminar a feature
```bash
# Voltar para develop
git checkout develop

# Puxar atualizações
git pull origin develop

# Fazer merge da feature
git merge feature/nome-da-feature

# Enviar para repositório
git push origin develop

# Deletar branch local
git branch -d feature/nome-da-feature

# Deletar branch remoto
git push origin --delete feature/nome-da-feature
```

---

## Commits

### Formato Simples
```
<tipo>: <descrição breve>

tipos recomendados:
- feat    (nova funcionalidade)
- fix     (correção de bug)
- refactor (reorganização de código)
- docs    (documentação)
- style   (formatação)
```

### Exemplos Bons
```
feat: adicionar criação de escalas
fix: corrigir validação de email
refactor: melhorar estrutura de componentes
docs: atualizar README com setup
```

### Exemplos Ruins
```
mudanças
atualizar código
corrigir
trabalho de hoje
```

---

## Merge para Main

Só fazer quando tiver **certeza** que está funcionando:

```bash
# Ir para main
git checkout main

# Puxar atualizações
git pull origin main

# Merge da develop
git merge develop

# Enviar
git push origin main

# Criar tag de versão (opcional)
git tag v1.0.0
git push origin v1.0.0
```

---

## Desfazer Mudanças

### Desfazer mudanças locais (antes de commit)
```bash
git restore .
```

### Desfazer último commit (sem perder código)
```bash
git reset --soft HEAD~1
```

### Desfazer último commit (perdendo código)
```bash
git reset --hard HEAD~1
```

### Ver histórico
```bash
git log --oneline
```

---

## Conflitos

Se houver conflito ao fazer merge:

### 1. Ver conflitos
```bash
git status
```

### 2. Abrir arquivo e resolver manualmente
```
<<<<<<< HEAD
seu código
=======
código do outro branch
>>>>>>> feature/nome
```

### 3. Remover marcadores e manter código correto

### 4. Fazer commit
```bash
git add .
git commit -m "fix: resolver conflito de merge"
```

---

## Comandos Úteis

```bash
# Ver branches
git branch -a

# Ver mudanças
git diff

# Ver commits recentes
git log --oneline -5

# Ver quem alterou linha
git blame arquivo.php

# Limpar branches deletados
git remote prune origin
```

---

## Fluxo Simplificado para TCC

```
1. git pull origin develop          # Pega mudanças
2. git checkout -b feature/xyz      # Cria feature
3. ... trabalha ...
4. git add .                         # Adiciona
5. git commit -m "feat: ..."        # Faz commit
6. git push origin feature/xyz       # Envia
7. git checkout develop              # Volta para develop
8. git merge feature/xyz             # Faz merge
9. git push origin develop           # Envia
```

