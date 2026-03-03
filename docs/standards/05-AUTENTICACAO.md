# 05 - AUTENTICAÇÃO

## Fluxo Simples com JWT

### 1. Usuário faz login
```
POST /api/v1/auth/login
{ email, password }
↓
Backend valida credenciais
↓
Retorna JWT Token
```

### 2. Frontend armazena token
```javascript
// localStorage
localStorage.setItem('token', 'eyJhbGciOiJIUzI1NiIs...')

// Ou cookie (mais seguro)
document.cookie = "token=eyJhbGciOiJIUzI1NiIs...;HttpOnly"
```

### 3. Frontend envia token em cada requisição
```javascript
// axios interceptor
axios.defaults.headers.common['Authorization'] = `Bearer ${token}`

// Ou no header da requisição
headers: {
  'Authorization': `Bearer ${token}`
}
```

### 4. Backend valida token
```php
// Middleware Laravel
Route::middleware('auth:sanctum')->group(function () {
    // Rotas protegidas
});
```

---

## Login Endpoint

### Request
```bash
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "usuario@example.com",
  "password": "senha123"
}
```

### Response (Sucesso)
```json
{
  "success": true,
  "message": "Login realizado com sucesso",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 1,
      "name": "João Silva",
      "email": "joao@example.com",
      "role": "employee"
    }
  }
}
```

### Response (Erro)
```json
{
  "success": false,
  "message": "Credenciais inválidas"
}
```

---

## Logout Endpoint

### Request
```bash
POST /api/v1/auth/logout
Authorization: Bearer TOKEN
```

### Response
```json
{
  "success": true,
  "message": "Logout realizado com sucesso"
}
```

---

## Roles (Funções)

### Três Roles Básicas
1. **admin** - Acesso total, gerencia tudo
2. **manager** - Gerencia escalas e usuários
3. **employee** - Vê suas escalas

### Verificar Role no Backend
```php
// No middleware
if (auth()->user()->role !== 'admin') {
    return response()->json(['error' => 'Unauthorized'], 403);
}
```

### Proteger Rotas por Role
```php
// routes/api.php
Route::middleware('auth:sanctum')->group(function () {
    // Admin only
    Route::post('/users', [UserController::class, 'store'])
        ->middleware('role:admin');
    
    // Manager or Admin
    Route::post('/schedules', [ScheduleController::class, 'store'])
        ->middleware('role:manager,admin');
});
```

---

## Frontend - Verificar Autenticação

### Exemplo com Pinia Store
```javascript
// stores/authStore.js
export const useAuthStore = defineStore('auth', () => {
  const user = ref(null)
  const token = ref(localStorage.getItem('token'))
  
  const login = async (email, password) => {
    const response = await axios.post('/api/v1/auth/login', {
      email,
      password
    })
    token.value = response.data.data.token
    user.value = response.data.data.user
    localStorage.setItem('token', token.value)
  }
  
  const logout = () => {
    user.value = null
    token.value = null
    localStorage.removeItem('token')
  }
  
  const isAuthenticated = computed(() => !!token.value)
  
  return { user, token, login, logout, isAuthenticated }
})
```

### Proteger Rotas no Frontend
```javascript
// router/index.js
import { useAuthStore } from '@/stores/authStore'

router.beforeEach((to, from, next) => {
  const authStore = useAuthStore()
  
  if (to.meta.requiresAuth && !authStore.isAuthenticated) {
    next('/login')
  } else {
    next()
  }
})
```

### Definir Rota Protegida
```javascript
// router/index.js
{
  path: '/dashboard',
  component: DashboardPage,
  meta: { requiresAuth: true }
}
```

---

## Backend - Form Request com Validação

```php
// app/Http/Requests/LoginRequest.php
namespace App\Http\Requests;

class LoginRequest extends FormRequest
{
    public function rules()
    {
        return [
            'email' => 'required|email',
            'password' => 'required|min:6'
        ];
    }
}
```

---

## Backend - Controller de Autenticação

```php
// app/Http/Controllers/AuthController.php
namespace App\Http\Controllers;

use Auth;

class AuthController extends Controller
{
    public function login(LoginRequest $request)
    {
        if (!Auth::attempt($request->validated())) {
            return response()->json(
                ['message' => 'Credenciais inválidas'],
                401
            );
        }
        
        $user = Auth::user();
        $token = $user->createToken('app-token')->plainTextToken;
        
        return response()->json([
            'success' => true,
            'message' => 'Login realizado com sucesso',
            'data' => [
                'token' => $token,
                'user' => $user
            ]
        ]);
    }
    
    public function logout()
    {
        Auth::user()->tokens()->delete();
        
        return response()->json([
            'success' => true,
            'message' => 'Logout realizado com sucesso'
        ]);
    }
}
```

---

## Resumo do Fluxo

1. User entra em `/login`
2. Digite email e senha
3. Frontend faz `POST /api/v1/auth/login`
4. Backend valida e retorna JWT token
5. Frontend armazena token no localStorage
6. Frontend adiciona token em todas as requisições
7. Backend valida token em cada requisição
8. User acessa páginas protegidas
9. Ao fazer logout, token é deletado

