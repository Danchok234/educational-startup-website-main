# Educational Startup Website

## Запуск через Python

Этот сайт статический, его можно быстро запустить встроенным HTTP-сервером Python.

### 1) Подключитесь к серверу

```bash
ssh user@your-server-ip
```

### 2) Перейдите в папку проекта

```bash
cd /path/to/educational-startup-website-main
```

### 3) Запуск в обычном режиме (для проверки)

```bash
python3 -m http.server 8080
```

Сайт будет доступен по адресу `http://SERVER_IP:8080`.

### 4) Запуск в фоне (для сервера)

```bash
nohup python3 -m http.server 8080 > server.log 2>&1 &
```

### 5) Проверка и остановка

```bash
curl -I http://localhost:8080
ps aux | grep "http.server 8080"
pkill -f "http.server 8080"
```

## Обновление сайта

```bash
cd /path/to/educational-startup-website-main
git pull
pkill -f "http.server 8080" || true
nohup python3 -m http.server 8080 > server.log 2>&1 &
```

## Docker (опционально)

Если нужен запуск в контейнере, используйте `Dockerfile` в корне проекта.

## GitHub Actions: автодеплой на сервер

При каждом `push` в ветку `master` workflow `/.github/workflows/deploy.yml` подключается к серверу по SSH и выполняет:

```bash
cd /var/www/educational-startup-website-main
git pull origin master
docker build -t educational-startup-website:latest .
docker rm -f educational-startup-website || true
docker run -d --name educational-startup-website --restart always -p 8080:8080 educational-startup-website:latest
```

### Что нужно добавить в Secrets репозитория

- `SERVER_HOST` - IP или домен сервера
- `SERVER_USER` - пользователь SSH
- `SERVER_SSH_KEY` - приватный SSH-ключ (в формате OpenSSH)
- `SERVER_PORT` - порт SSH (обычно `22`)

### Что должно быть на сервере

- Установлены `git` и `docker`
- Проект уже склонирован в `/var/www/educational-startup-website-main`
- У пользователя, указанного в `SERVER_USER`, есть доступ к Docker


