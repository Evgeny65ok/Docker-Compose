# 🐳 Самостоятельные проекты на Dockerfile

Лабораторное задание: **создать несколько собственных проектов с использованием Dockerfile** для сборки кастомных образов.

Каждый проект собирает **собственный образ** через `Dockerfile` на базе официального образа, автоматически добавляя недостающие системные утилиты и настраивая `HEALTHCHECK` для мониторинга состояния контейнеров.

---

## 📋 Список проектов

| № | Проект | Описание | Локальный порт |
| :---: | :--- | :--- | :---: |
| **1** | [postgresql-adminer](./postgresql-adminer/) | PostgreSQL 17 + Adminer (универсальный веб-клиент для БД) | `8080` |
| **2** | [postgresql-pgadmin-dockerfile](./postgresql-pgadmin-dockerfile/) | PostgreSQL 17 + pgAdmin 4 (официальный веб-интерфейс) | `5051` |
| **3** | [postgresql-cloudbeaver-dockerfile](./postgresql-cloudbeaver-dockerfile/) | PostgreSQL 17 + CloudBeaver (веб-версия DBeaver) | `8979` |

---

## 🏗️ Что делает каждый Dockerfile

1. **PostgreSQL + Adminer**
   * **Dockerfile:** Основан на `adminer:latest` + установка PHP-драйвера PostgreSQL (`php83-pgsql`) + проверка работоспособности через `wget`.
2. **PostgreSQL + pgAdmin**
   * **Dockerfile:** Основан на `dpage/pgadmin4:latest` + доустановка `wget` и `curl` для корректной работы встроенного `HEALTHCHECK`.
3. **PostgreSQL + CloudBeaver**
   * **Dockerfile:** Основан на `dbeaver/cloudbeaver:latest` + установка `curl` в дистрибутив Ubuntu/Debian для регулярной проверки доступности веб-сервера.

---

## 🚀 Управление проектами

### 1. Запуск любого проекта
Перейдите в директорию нужного проекта и соберите контейнеры:
```bash
cd <имя-проекта>
docker compose up -d --build
```
> ⚠️ **Важно:** Флаг `--build` обязателен при первом запуске и изменениях — без него Docker не соберёт кастомный образ из вашего `Dockerfile`.

### 2. Проверка статуса контейнеров
Убедитесь, что все контейнеры успешно запустились и прошли проверку healthcheck (статус `healthy`):
```bash
docker compose ps -a
```

### 3. Остановка сервисов
Мягкая остановка и удаление контейнеров текущего проекта:
```bash
docker compose down
```

### 4. Полная очистка
Удаление контейнеров вместе с созданными томами (данными БД) и локально собранными образами:
```bash
docker compose down --rmi all -v
```

---

## 📁 Структура директории

```text
dockerfile-projects/
├── README.md
├── postgresql-adminer/
│   ├── Dockerfile
│   ├── compose.yaml
│   ├── README.md
│   └── *.png
├── postgresql-pgadmin-dockerfile/
│   ├── Dockerfile
│   ├── compose.yaml
│   ├── README.md
│   └── *.png
└── postgresql-cloudbeaver-dockerfile/
    ├── Dockerfile
    ├── compose.yaml
    ├── README.md
    └── *.png
```

---

## 🧰 Системные требования

* Установленный **Docker Desktop** (включающий Docker Engine и Compose v2).
* Свободные сетевые порты на хост-машине: `8080`, `5051`, `8979`.
