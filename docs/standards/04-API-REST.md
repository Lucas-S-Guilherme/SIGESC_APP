# 04 - API REST

## Endpoints Básicos

### Authentication
```
POST   /api/v1/auth/login         # Fazer login
POST   /api/v1/auth/logout        # Fazer logout
POST   /api/v1/auth/refresh       # Renovar token
```

### Users
```
GET    /api/v1/users              # Listar usuários
POST   /api/v1/users              # Criar usuário
GET    /api/v1/users/:id          # Obter um usuário
PUT    /api/v1/users/:id          # Atualizar usuário
DELETE /api/v1/users/:id          # Deletar usuário
```

### Schedules
```
GET    /api/v1/schedules          # Listar escalas
POST   /api/v1/schedules          # Criar escala
GET    /api/v1/schedules/:id      # Obter escala
PUT    /api/v1/schedules/:id      # Atualizar escala
DELETE /api/v1/schedules/:id      # Deletar escala
```

### Shifts
```
GET    /api/v1/shifts             # Listar turnos
POST   /api/v1/shifts             # Criar turno
GET    /api/v1/shifts/:id         # Obter turno
PUT    /api/v1/shifts/:id         # Atualizar turno
DELETE /api/v1/shifts/:id         # Deletar turno
```

---

## Formato de Resposta Sucesso

```json
{
  "success": true,
  "message": "Operação realizada com sucesso",
  "data": {
    "id": 1,
    "name": "Escala Janeiro",
    "created_at": "2026-03-02T10:30:00Z"
  }
}
```

### Lista com Paginação
```json
{
  "success": true,
  "message": "Dados obtidos com sucesso",
  "data": [
    { "id": 1, "name": "..." },
    { "id": 2, "name": "..." }
  ],
  "meta": {
    "total": 100,
    "per_page": 15,
    "current_page": 1,
    "last_page": 7
  }
}
```

---

## Formato de Resposta Erro

### Erro de Validação
```json
{
  "success": false,
  "message": "Dados inválidos",
  "errors": {
    "email": ["Email já existe", "Email deve ser válido"],
    "name": ["Nome é obrigatório"]
  }
}
```

### Erro Geral
```json
{
  "success": false,
  "message": "Não foi possível processar a requisição",
  "error": "Descrição do erro"
}
```

---

## HTTP Status Codes

| Código | Significado | Quando usar |
|--------|-------------|------------|
| 200 | OK | GET/PUT bem-sucedido |
| 201 | Created | POST bem-sucedido (criou) |
| 400 | Bad Request | Dados inválidos |
| 401 | Unauthorized | Não autenticado |
| 403 | Forbidden | Sem permissão |
| 404 | Not Found | Recurso não existe |
| 422 | Unprocessable Entity | Erro de validação |
| 500 | Server Error | Erro no servidor |

---

## Exemplo: Criar Escala

### Request
```bash
POST /api/v1/schedules
Content-Type: application/json
Authorization: Bearer TOKEN

{
  "name": "Escala Março",
  "start_date": "2026-03-01",
  "end_date": "2026-03-31"
}
```

### Response (201)
```json
{
  "success": true,
  "message": "Escala criada com sucesso",
  "data": {
    "id": 1,
    "name": "Escala Março",
    "start_date": "2026-03-01",
    "end_date": "2026-03-31",
    "created_at": "2026-03-02T10:30:00Z"
  }
}
```

---

## Exemplo: Validação Falhar

### Request
```bash
POST /api/v1/schedules
Content-Type: application/json

{
  "name": ""
}
```

### Response (422)
```json
{
  "success": false,
  "message": "Erro de validação",
  "errors": {
    "name": ["Campo obrigatório"],
    "start_date": ["Data obrigatória"],
    "end_date": ["Data obrigatória"]
  }
}
```

---

## Query Parameters

### Paginação
```
GET /api/v1/schedules?page=2&per_page=10
```

### Filtros
```
GET /api/v1/schedules?status=active&user_id=5
```

### Ordenação
```
GET /api/v1/schedules?sort=created_at&order=desc
```

### Busca
```
GET /api/v1/schedules?search=Escala%20Janeiro
```

