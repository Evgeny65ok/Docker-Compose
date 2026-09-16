# 🐳 Лабораторная работа: PostgreSQL + CloudBeaver через Dockerfile

**CloudBeaver** — это мощная веб-версия популярного десктопного инструмента DBeaver для управления базами данных. В данном проекте CloudBeaver **собран из собственного Dockerfile** на базе официального образа `dbeaver/cloudbeaver` с добавлением утилиты `curl` для проверки состояния контейнера (healthcheck).

---

### 🌐 Сетевые параметры
* **Локальный порт CloudBeaver:** `8979` (внешний)
* **Внутренний порт CloudBeaver:** `8978`
* **Внутренний порт PostgreSQL:** `5432`

---

## 🏗️ 1. Описание проекта и конфигурация

Проект состоит из двух связанных сервисов:
* 🗄️ **db** — СУБД PostgreSQL 17-alpine
* 🐳 **cloudbeaver** — веб-интерфейс, собранный из кастомного `Dockerfile`

### 📄 Dockerfile
```dockerfile
FROM dbeaver/cloudbeaver:latest
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
    && rm -rf /var/lib/apt/lists/*
EXPOSE 8978
HEALTHCHECK --interval=30s --timeout=10s --retries=3 --start-period=60s \
  CMD curl -f http://localhost:8978/ || exit 1
```

### 📂 Структура проекта
```text
postgresql-cloudbeaver-dockerfile/
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
![Успешная сборка и запуск](1.png)

---

## 🖥️ 3. Первичная настройка CloudBeaver

Откройте браузер и перейдите по адресу: [http://localhost:8979](http://localhost:8979)

При первом входе система предложит создать учётную запись администратора, после чего откроется панель базовых настроек **Server Configuration**.

### 📸 Скриншот 2: Настройки CloudBeaver
![Настройки CloudBeaver](2.png)

---

## 🗄️ 4. Подключение к PostgreSQL

Для работы с базой данных добавьте новое подключение (**New Connection**) к серверу PostgreSQL, используя следующие параметры:

### ⚙️ Параметры подключения к БД:

| Настройка | Значение |
| :--- | :--- |
| **Host** | `db` |
| **Port** | `5432` |
| **Database** | `mydatabase` |
| **User** | `myuser` |

После сохранения в левой панели (навигаторе) отобразится подключение `PostgreSQL@db`, предоставляющее полный доступ к таблицам и схеме базы `mydatabase`.

### 📸 Скриншот 3: База данных в CloudBeaver
![База данных в CloudBeaver](3.png)
