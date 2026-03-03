# 02 - CONVENÇÕES DE NOMENCLATURA

## Backend (PHP/Laravel)

### Classes
- PascalCase
- Exemplos: `UserController`, `ScheduleRequest`, `UserModel`

### Métodos e Funções
- camelCase
- Exemplos: `getUsers()`, `createSchedule()`, `updateUser()`

### Variáveis
- camelCase
- Exemplos: `$userData`, `$isActive`, `$userId`

### Constantes
- UPPER_SNAKE_CASE
- Exemplos: `MAX_SHIFTS`, `DEFAULT_TIMEZONE`

### Models
- Singular, PascalCase
- Exemplos: `User`, `Schedule`, `Shift`

### Controllers
- Recurso + Controller
- Exemplos: `UserController`, `ScheduleController`

### Services
- Noun + Service
- Exemplos: `ScheduleService`, `UserService`

### Migrations
- `YYYY_MM_DD_HHMMSS_description.php`
- Exemplo: `2026_03_02_100000_create_users_table.php`

### Form Requests
- Ação + Recurso + Request
- Exemplos: `StoreUserRequest`, `UpdateScheduleRequest`

---

## Frontend (JavaScript/Vue.js)

### Componentes Vue
- PascalCase
- Arquivo: `UserCard.vue`, `ScheduleList.vue`

### Páginas
- PascalCase + Page
- Arquivo: `UsersPage.vue`, `DashboardPage.vue`

### Stores (Pinia)
- camelCase com sufixo "Store"
- Arquivo: `userStore.js`, `scheduleStore.js`

### Serviços
- camelCase + Service
- Arquivo: `userService.js`, `apiService.js`

### Composables
- useXxx format
- Arquivo: `useAuth.js`, `useFetch.js`, `useSchedules.js`

### Variáveis
- camelCase
- Exemplos: `isLoading`, `userData`, `errorMessage`

### Constantes
- UPPER_SNAKE_CASE
- Exemplos: `API_BASE_URL`, `MAX_RETRIES`

### Funções auxiliares
- camelCase
- Exemplos: `formatDate()`, `getFullName()`

### Classes CSS
- kebab-case
- Exemplos: `user-card`, `schedule-grid`, `modal-overlay`

---

## Database

### Tabelas
- snake_case, plural
- Exemplos: `users`, `schedules`, `shifts`

### Colunas
- snake_case
- Exemplos: `first_name`, `created_at`, `is_active`

### Foreign Keys
- singular_id
- Exemplos: `user_id`, `schedule_id`

### Timestamps
- Sempre usar `created_at`, `updated_at`, `deleted_at`

---

## Git Commits

### Formato Simples
```
<tipo>: <descrição em português>

tipos: feat, fix, refactor, docs, style
```

### Exemplos
- `feat: adicionar criação de escalas`
- `fix: corrigir validação de email`
- `refactor: melhorar estrutura de componentes`
- `docs: atualizar README`

