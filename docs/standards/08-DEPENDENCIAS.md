# 08 - DEPENDÊNCIAS PRINCIPAIS

## Backend (Laravel)

### Versões Essenciais

```json
{
  "require": {
    "php": "^8.1",
    "laravel/framework": "^10.0",
    "laravel/sanctum": "^3.0",
    "laravel/tinker": "^2.0"
  }
}
```

### Instalação
```bash
# Dentro do container
docker-compose exec app composer install

# Ou localmente (com Composer instalado)
cd backend && composer install
```

### Principais Packages
- **laravel/framework** - Framework principal
- **laravel/sanctum** - Autenticação via tokens/cookies
- **laravel/tinker** - Console interativo

---

## Frontend (Node.js)

### Versões Essenciais

```json
{
  "dependencies": {
    "vue": "^3.3",
    "axios": "^1.4",
    "pinia": "^2.1",
    "vue-router": "^4.2"
  },
  "devDependencies": {
    "vite": "^4.3",
    "@vitejs/plugin-vue": "^4.2",
    "eslint": "^8.0",
    "prettier": "^3.0"
  }
}
```

### Instalação
```bash
cd frontend && npm install
```

### Principais Packages
- **vue** - Framework frontend
- **axios** - HTTP client (chamadas API)
- **pinia** - State management
- **vue-router** - Roteamento
- **vite** - Build tool (compilador)

---

## Instalação no Docker

### Backend
```bash
docker-compose exec app composer install
```

### Frontend (deve rodar local, não no Docker)
```bash
cd frontend
npm install
npm run dev
```

---

## Scripts npm (Frontend)

No arquivo `package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint src/",
    "format": "prettier --write ."
  }
}
```

### Usar:
```bash
npm run dev       # Inicia servidor de desenvolvimento
npm run build     # Faz build para produção
npm run lint      # Verifica estilo de código
npm run format    # Formata código
```

---

## Scripts Artisan (Backend)

No Laravel, usar `php artisan`:

```bash
# Dentro do container
docker-compose exec app php artisan <comando>

# Principais:
php artisan migrate              # Executa migrações
php artisan db:seed              # Popula banco com dados
php artisan make:model User      # Cria novo Model
php artisan make:controller      # Cria novo Controller
php artisan make:request         # Cria novo Form Request
php artisan tinker               # Console interativo
php artisan serve                # Inicia servidor
```

---

## Arquivo .gitignore

Arquivos que **não** devem ser enviados para Git:

```
# Backend
/backend/vendor/
/backend/.env
/backend/.env.local
/backend/storage/logs/*
/backend/storage/app/*
/backend/bootstrap/cache/*

# Frontend
/frontend/node_modules/
/frontend/.env
/frontend/dist/

# Geral
.DS_Store
*.log
.vscode/*
.idea/*
```

---

## Como Adicionar Nova Dependência

### Backend
```bash
docker-compose exec app composer require vendor/package
```

### Frontend
```bash
cd frontend
npm install package-name
```

---

## Dependências Opcionais (Não Essenciais)

### Backend
- `laravel/pint` - Formatação automática de código
- `laravel/horizon` - Dashboard para filas
- `spatie/laravel-permission` - Sistema de permissões avançado

### Frontend
- `tailwindcss` - Framework CSS
- `vee-validate` - Validação de formulários
- `date-fns` - Manipulação de datas

