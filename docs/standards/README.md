# 📚 Padrões do SIGESBM

Documentação fragmentada e focada no essencial para o TCC.

## 📖 Índice de Documentos

| # | Documento | Descrição |
|---|-----------|-----------|
| **01** | [ESTRUTURA.md](01-ESTRUTURA.md) | Organização de pastas do projeto |
| **02** | [NOMENCLATURA.md](02-NOMENCLATURA.md) | Convenções de nomes (PHP, JS, SQL) |
| **03** | [ARQUITETURA.md](03-ARQUITETURA.md) | Padrões de design (Controllers, Services, Stores) |
| **04** | [API-REST.md](04-API-REST.md) | Endpoints, respostas, HTTP status codes |
| **05** | [AUTENTICACAO.md](05-AUTENTICACAO.md) | Login, JWT tokens, roles |
| **06** | [DOCKER.md](06-DOCKER.md) | Containers, docker-compose, setup inicial |
| **07** | [GIT.md](07-GIT.md) | Branches, commits, workflow |
| **08** | [DEPENDENCIAS.md](08-DEPENDENCIAS.md) | Packages e instalação |

---

## ⚡ Início Rápido

### 1. Clonar e setup
```bash
git clone https://github.com/seu-usuario/SIGESBM.git
cd SIGESBM
docker-compose up -d
```

### 2. Instalar dependências
```bash
docker-compose exec app composer install
cd frontend && npm install
```

### 3. Configurar banco
```bash
docker-compose exec app php artisan migrate
docker-compose exec app php artisan db:seed
```

### 4. Iniciar
- Backend: http://localhost
- Frontend: `cd frontend && npm run dev`

---

## 🎯 O Que Está Aqui

✅ **Leve e objetivo**  
✅ **Sem testes, CI/CD (por enquanto)**  
✅ **Focado em implementação rápida**  
✅ **Tudo que você precisa saber para começar**  
✅ **Fácil expandir depois**

---

## 📋 Checklist Básico para Implementar Feature

1. Ler [02-NOMENCLATURA.md](02-NOMENCLATURA.md) para entender nomes
2. Ler [03-ARQUITETURA.md](03-ARQUITETURA.md) para entender estrutura
3. Ler [04-API-REST.md](04-API-REST.md) para endpoints
4. Implementar Model → Migration → Controller → Routes (backend)
5. Implementar Service → Component/Page → Store (frontend)
6. Testar manualmente
7. Fazer commit seguindo [07-GIT.md](07-GIT.md)

---

## 💡 Dicas

- Leia um documento por vez
- Não precisa memorizar tudo
- Use como referência durante o desenvolvimento
- Pergunte à IA qualquer coisa que não entender
- Estes padrões podem ser ajustados conforme necessário

---

**Última atualização:** 2 de março de 2026  
**Versão:** 2.0 - TCC Essencial

