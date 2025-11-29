# 🐳 Docker Guide

## Быстрый старт

### Вариант 1: Docker Compose (рекомендуется)

```bash
# Собрать и запустить
docker-compose up --build

# В фоновом режиме
docker-compose up -d

# Остановить
docker-compose down
```

### Вариант 2: Docker команды

```bash
# Собрать образ
docker build -t coffeeshop-backend:latest .

# Запустить контейнер
docker run -d \
  --name coffeeshop-backend \
  -p 8080:8080 \
  coffeeshop-backend:latest

# Остановить
docker stop coffeeshop-backend

# Удалить контейнер
docker rm coffeeshop-backend
```

## 📋 Что внутри Dockerfile

### Stage 1: Build (Maven)
- Базовый образ: `maven:3.9.6-eclipse-temurin-17`
- Копирует Maven wrapper и `pom.xml`
- Загружает зависимости (кэшируется)
- Копирует исходники
- Собирает JAR файл

### Stage 2: Runtime (JRE)
- Базовый образ: `eclipse-temurin:17-jre-alpine` (лёгкий!)
- Копирует только JAR файл из build stage
- Размер образа: ~250MB (вместо 700MB с Maven)
- Запускает приложение

## 🔍 Проверка работы

```bash
# Проверить логи
docker logs coffeeshop-backend

# Следить за логами
docker logs -f coffeeshop-backend

# Проверить здоровье
curl http://localhost:8080/actuator/health

# Проверить каталог
curl http://localhost:8080/catalog
```

## 🎯 Endpoints в Docker

После запуска доступны на `http://localhost:8080`:

- `GET /catalog` - полный каталог
- `GET /menu` - простое меню
- `POST /order` - создать простой заказ
- `POST /order/custom` - создать кастомный заказ

## 🚀 Production готовность

### Health Check
Контейнер автоматически проверяет здоровье каждые 30 секунд:
```yaml
healthcheck:
  test: wget http://localhost:8080/actuator/health
  interval: 30s
  timeout: 3s
  start-period: 40s
```

### Restart Policy
Автоматически перезапускается при сбоях:
```yaml
restart: unless-stopped
```

## 📦 Размеры образов

| Stage | Образ | Размер |
|-------|-------|--------|
| Build | maven:3.9.6-eclipse-temurin-17 | ~700MB |
| Runtime | eclipse-temurin:17-jre-alpine | ~250MB |
| **Final** | coffeeshop-backend:latest | **~250MB** |

## 🔧 Переменные окружения

```bash
docker run -d \
  -e SPRING_PROFILES_ACTIVE=prod \
  -e SERVER_PORT=8080 \
  -p 8080:8080 \
  coffeeshop-backend:latest
```

## 🐛 Troubleshooting

### mvnw: Permission denied
Если при сборке возникает ошибка `Permission denied` для `mvnw`:
```dockerfile
# Добавьте в Dockerfile перед RUN ./mvnw
RUN chmod +x mvnw
```
Это уже исправлено в текущей версии!

### Контейнер не запускается
```bash
# Проверить логи
docker logs coffeeshop-backend

# Зайти внутрь контейнера
docker exec -it coffeeshop-backend sh

# Проверить порты
docker ps
```

### Порт занят
```bash
# Использовать другой порт
docker run -p 9090:8080 coffeeshop-backend:latest
```

### Пересобрать без кэша
```bash
docker build --no-cache -t coffeeshop-backend:latest .
```

## 📝 .dockerignore

Файл `.dockerignore` исключает ненужные файлы из контекста сборки:
- `target/` - Maven build артефакты
- `node_modules/` - Node.js зависимости
- `.git/` - Git репозиторий
- `*.md` - Документация

Это ускоряет сборку и уменьшает размер контекста!

## 🌐 Docker Hub (опционально)

### Залить образ на Docker Hub
```bash
# Логин
docker login

# Тегировать
docker tag coffeeshop-backend:latest username/coffeeshop-backend:latest

# Отправить
docker push username/coffeeshop-backend:latest
```

### Скачать и запустить
```bash
docker pull username/coffeeshop-backend:latest
docker run -p 8080:8080 username/coffeeshop-backend:latest
```

## ✅ Best Practices

1. ✅ **Multi-stage build** - уменьшает размер
2. ✅ **Layer caching** - ускоряет пересборку
3. ✅ **Alpine Linux** - минимальный размер
4. ✅ **Health checks** - автоматический мониторинг
5. ✅ **.dockerignore** - быстрая сборка
6. ✅ **Non-root user** (можно добавить)

## 🔐 Security (опционально)

Добавить non-root пользователя:

```dockerfile
# В Runtime stage
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
```

---

**Готово!** Теперь ваше приложение готово к деплою в любое окружение с Docker! 🎉
