# 01 - ESTRUTURA DE DIRETÓRIOS

## Estrutura Principal

```
SIGESBM/
├── backend/              # Código Laravel
├── frontend/             # Código Vue.js
├── docker/               # Arquivos Docker
├── docs/                 # Documentação
├── docker-compose.yml    # Orquestração containers
├── .env.example          # Exemplo de variáveis
├── .gitignore            # Gitignore
└── README.md             # Documentação principal
```

---

## Backend (Laravel)

```
backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/      # Controllers REST
│   │   ├── Requests/         # Form Requests (validação)
│   │   └── Resources/        # API Resources (response format)
│   ├── Models/               # Modelos Eloquent
│   └── Services/             # Serviços (lógica de negócio)
├── database/
│   ├── migrations/           # Migrações
│   └── seeders/              # Dados iniciais
├── routes/
│   └── api.php               # Rotas da API
├── config/                   # Configuração
├── .env.example              # Variáveis de ambiente
├── composer.json             # Dependências
└── README.md                 # Docs
```

---

## Frontend (Vue.js)

```
frontend/
├── src/
│   ├── components/           # Componentes reutilizáveis
│   │   ├── Common/           # Botões, inputs, etc
│   │   └── Features/         # Componentes de features
│   ├── pages/                # Páginas/telas
│   ├── stores/               # Pinia stores (state)
│   ├── services/             # Serviços (API calls)
│   ├── utils/                # Funções auxiliares
│   ├── assets/               # Imagens, estilos
│   ├── router/               # Vue Router config
│   ├── App.vue               # Componente raiz
│   └── main.js               # Entry point
├── package.json              # Dependências
├── vite.config.js            # Configuração Vite
└── README.md                 # Docs
```

---

## Para Começar

1. Clone o repositório
2. Entre em cada pasta (backend e frontend)
3. Instale dependências
4. Configure .env
5. Rode com Docker

