# 🐳 Лабораторная работа: PostgreSQL + pgAdmin через Dockerfile

**pgAdmin 4** — официальный и самый популярный веб-клиент для администрирования СУБД PostgreSQL. В данном проекте pgAdmin **собран из собственного Dockerfile** на базе официального образа `dpage/pgadmin4` с добавлением утилит для проверки состояния контейнера (healthcheck).

---

### 🌐 Сетевые параметры
* **Локальный порт pgAdmin:** `5051`
* **Внутренний порт PostgreSQL:** `5432`

---

## 🏗️ 1. Описание проекта и конфигурация

Проект состоит из двух связанных сервисов:
* 🗄️ **db** — СУБД PostgreSQL 17-alpine
* 🐳 **pgadmin** — веб-интерфейс, собранный из кастомного `Dockerfile`

### 📄 Dockerfile
```dockerfile
FROM dpage/pgadmin4:latest
USER root
RUN apk add --no-cache wget curl || true
EXPOSE 80
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=40s \
  CMD wget --spider -q http://localhost:80/misc/ping || exit 1
```

### 📂 Структура проекта
```text
postgresql-pgadmin-dockerfile/
├── README.md
├── Dockerfile
├── compose.yaml
├── 1.png
├── 2.png
└── 3.png
```

---

## 🚀 2. Сборка и запуск

Выполните команду в терминале для сборки образов и запуска контейнеров в фоновом режиме:

```bash
docker compose up -d --build
```

### 📸 Скриншот 1: Успешная сборка и запуск
![alt text](<Снимок экрана 2026-09-16 111453.png>)
---

## 🖥️ 3. Вход в pgAdmin

Откройте браузер и перейдите по адресу: [http://localhost:5051](http://localhost:5051)

### 🔑 Данные для авторизации в веб-интерфейсе:

| Параметр | Значение |
| :--- | :--- |
| **Email** | `admin@example.com` |
| **Password** | `admin` |

### 📸 Скриншот 2: Подключение pgAdmin к PostgreSQL
![alt text](<Снимок экрана 2026-09-16 111619.png>)
---

## 🗄️ 4. База данных в pgAdmin

При добавлении нового сервера (Register -> Server) используйте следующие параметры для подключения к контейнеру с базой данных:

### ⚙️ Параметры подключения к БД:

| Настройка | Значение |
| :--- | :--- |
| **Host name/address** | `db` |
| **Port** | `5432` |
| **Maintenance database** | `mydatabase` |
| **Username** | `myuser` |

### 📸 Скриншот 3: База данных в pgAdmin
![alt text](<Снимок экрана 2026-09-16 111817.png>)