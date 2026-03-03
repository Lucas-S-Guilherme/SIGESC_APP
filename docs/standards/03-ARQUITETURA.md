# 03 - PADRÕES DE ARQUITETURA

## Backend - 3 Camadas Simples

```
Request (HTTP)
    ↓
Controller (recebe dados, valida básico)
    ↓
Service (lógica de negócio)
    ↓
Model (acesso dados com Eloquent)
    ↓
Database (MySQL)
```

---

## Controllers

Responsabilidade: receber requisição, chamar Service, retornar resposta

```php
// app/Http/Controllers/ScheduleController.php
namespace App\Http\Controllers;

class ScheduleController extends Controller
{
    public function index()      // GET /schedules
    public function store()      // POST /schedules
    public function show($id)    // GET /schedules/{id}
    public function update($id)  // PUT /schedules/{id}
    public function destroy($id) // DELETE /schedules/{id}
}
```

---

## Services

Responsabilidade: conter toda a lógica de negócio

```php
// app/Services/ScheduleService.php
class ScheduleService
{
    public function createSchedule(array $data)
    public function updateSchedule($id, array $data)
    public function getActiveSchedules()
    public function assignUserToShift($userId, $shiftId)
}
```

---

## Models

Responsabilidade: representar dados + relações

```php
// app/Models/Schedule.php
class Schedule extends Model
{
    protected $fillable = ['name', 'start_date', 'end_date'];
    
    public function users() { /* relação */ }
    public function shifts() { /* relação */ }
}
```

---

## Frontend - Componentes + Estado

```
Pages (telas completas)
    ↓
Components (reutilizáveis)
    ↓
Stores (Pinia - estado compartilhado)
    ↓
Services (chamadas à API)
    ↓
API (backend)
```

---

## Components

Componentes pequenos e reutilizáveis

```vue
<!-- UserCard.vue -->
<template>
  <div class="user-card">
    <h3>{{ user.name }}</h3>
    <button @click="$emit('edit')">Editar</button>
  </div>
</template>

<script setup>
defineProps({
  user: Object
})
</script>
```

---

## Pages

Páginas completas que combinam componentes

```vue
<!-- pages/SchedulesPage.vue -->
<template>
  <div>
    <h1>Escalas</h1>
    <ScheduleList :schedules="schedules" />
    <CreateScheduleForm @submit="handleCreate" />
  </div>
</template>

<script setup>
import { useScheduleStore } from '@/stores/scheduleStore'
const store = useScheduleStore()
const schedules = store.schedules
</script>
```

---

## Stores (Pinia)

Estado centralizado

```javascript
// stores/scheduleStore.js
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useScheduleStore = defineStore('schedule', () => {
  const schedules = ref([])
  
  const activeSchedules = computed(
    () => schedules.value.filter(s => s.is_active)
  )
  
  const fetchSchedules = async () => {
    const data = await scheduleService.getSchedules()
    schedules.value = data
  }
  
  return { schedules, activeSchedules, fetchSchedules }
})
```

---

## Services

Chamadas à API isoladas

```javascript
// services/scheduleService.js
import axios from 'axios'

export const scheduleService = {
  async getSchedules() {
    const response = await axios.get('/api/v1/schedules')
    return response.data.data
  },
  
  async createSchedule(data) {
    const response = await axios.post('/api/v1/schedules', data)
    return response.data.data
  }
}
```

---

## Composables

Lógica reutilizável entre componentes

```javascript
// composables/useSchedules.js
import { ref } from 'vue'
import { scheduleService } from '@/services/scheduleService'

export function useSchedules() {
  const schedules = ref([])
  const loading = ref(false)
  
  const loadSchedules = async () => {
    loading.value = true
    schedules.value = await scheduleService.getSchedules()
    loading.value = false
  }
  
  return { schedules, loading, loadSchedules }
}
```

---

## Fluxo de Dados

### Backend
```
POST /api/v1/schedules
├─ ScheduleController::store()
├─ ScheduleService::createSchedule()
├─ Schedule::create()
└─ return JSON
```

### Frontend
```
User clica "Criar Escala"
├─ scheduleService.createSchedule(dados)
├─ axios POST para backend
├─ Store é atualizado
├─ Componente renderiza novo estado
└─ User vê a escala criada
```

