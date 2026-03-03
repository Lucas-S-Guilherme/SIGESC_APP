# 06 - DOCKER

## O que é Docker

Cria containers (ambientes isolados) para rodar a aplicação com tudo que precisa:
- PHP + Laravel
- Nginx (servidor web)
- MySQL (banco dados)

---

## docker-compose.yml

Arquivo que define todos os containers. Exemplo básico:

```yaml
version: '3.8'

services:
  # Servidor Nginx
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./backend:/var/www/html
      - ./docker/nginx:/etc/nginx/conf.d
    depends_on:
      - app

  # PHP-FPM (Laravel)
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./backend:/var/www/html
    environment:
      - DB_HOST=db
      - DB_DATABASE=sigesbm
      - DB_USERNAME=root
      - DB_PASSWORD=secret
    depends_on:
      - db

  # MySQL
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: sigesbm
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

---

## Dockerfile

Define como construir a imagem Docker:

```dockerfile
FROM php:8.1-fpm

# Instalar extensões PHP necessárias
RUN docker-php-ext-install pdo pdo_mysql

# Instalar Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copiar código
WORKDIR /var/www/html
COPY backend .

# Instalar dependências
RUN composer install

# Permissões
RUN chown -R www-data:www-data /var/www/html
```

---

## Comandos Básicos

### Iniciar containers
```bash
docker-compose up -d
```

### Ver logs
```bash
docker-compose logs -f app
```

### Parar containers
```bash
docker-compose down
```

### Executar comando dentro do container
```bash
docker-compose exec app php artisan migrate
```

---

## Setup Inicial

### 1. Build e inicia
```bash
docker-compose up -d
```

### 2. Instalar dependências PHP
```bash
docker-compose exec app composer install
```

### 3. Gerar chave da aplicação
```bash
docker-compose exec app php artisan key:generate
```

### 4. Executar migrações
```bash
docker-compose exec app php artisan migrate
```

### 5. Semear banco (dados iniciais)
```bash
docker-compose exec app php artisan db:seed
```

### 6. Acesse em http://localhost

---

## Variáveis de Ambiente (.env)

Criar arquivo `.env` na raiz do projeto:

```
APP_NAME=SIGESBM
APP_ENV=local
APP_DEBUG=true
APP_KEY=
APP_URL=http://localhost

# Database
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=sigesbm
DB_USERNAME=root
DB_PASSWORD=secret

# Frontend
VITE_API_URL=http://localhost/api
```

---

## Estrutura Docker

```
docker/
├── nginx/
│   └── default.conf      # Config do servidor web
└── mysql/
    └── my.cnf             # Config do MySQL
```

### Nginx Config Exemplo
```nginx
server {
    listen 80;
    server_name localhost;

    root /var/www/html/public;
    index index.php;

    location ~ \.php$ {
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
}
```

---

## Dicas

- Sempre use `docker-compose exec` dentro do container
- Use `docker-compose logs` para ver o que está acontecendo
- Se não funcionar, tente `docker-compose down && docker-compose up -d`
- Volumes persistem dados (não são deletados)

