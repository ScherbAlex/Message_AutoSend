# 🚀 Message_AutoSend — Email Campaign Management Platform (Django)

### Production-ready Django application for managing email campaigns, recipients, messages, scheduling and delivery reports.

Проект демонстрирует **архитектуру backend-приложения**, работу с фоновыми задачами, кешированием, логированием и деплоем через Docker.

![Python](https://img.shields.io/badge/Python-3.12%2B-blue)
![Django](https://img.shields.io/badge/Django-5.2%2B-green)
![PostgreSQL](https://img.shields.io/badge/DB-PostgreSQL-blue)
![Redis](https://img.shields.io/badge/Cache-Redis-red)
![Celery](https://img.shields.io/badge/Tasks-Celery-brightgreen)
![Docker](https://img.shields.io/badge/Deploy-Docker-blue)
![CI](https://img.shields.io/badge/CI-GitHub_Actions-black)

---

## 📌 About the project

**Message_AutoSend** — это backend-проект уровня production, реализующий:

- управление получателями, письмами и рассылками
- автоматическую отправку по расписанию
- фоновую обработку задач
- детальное логирование и аудит
- серверное и клиентское кеширование
- деплой через Docker Compose

Проект построен **без упрощений**, с упором на:
- читаемую архитектуру
- расширяемость
- реальные сценарии эксплуатации

---

## 🧠 Architecture overview

```mermaid
flowchart TD
    Browser[Client / Browser]
    Nginx[Nginx]
    Django[Django App<br/>Gunicorn]
    DB[(PostgreSQL)]
    Redis[(Redis)]
    CeleryWorker[Celery Worker]
    CeleryBeat[Celery Beat / APScheduler]
    SMTP[SMTP Server]

    Browser -->|HTTP| Nginx
    Nginx --> Django

    Django --> DB
    Django --> Redis

    Django -->|enqueue tasks| Redis
    Redis --> CeleryWorker
    CeleryWorker --> SMTP

    CeleryBeat -->|scheduled tasks| Redis
🔧 Tech stack
Python 3.12+

Django 5.2+

PostgreSQL 14+

Redis (cache + message broker)

Celery (background tasks)

django-apscheduler (task scheduling)

Gunicorn + Nginx

Docker / Docker Compose

GitHub Actions (CI/CD)

📁 Project structure
Message_AutoSend/
├─ config/                  # Django settings, URLs, WSGI/ASGI
├─ common/                  # Middleware, mixins, logging helpers
│  ├─ middleware.py         # Request context (request_id, user)
│  ├─ mixins.py             # ClientCacheMixin (HTTP caching)
│  └─ logging_filters.py
├─ clients/                 # Recipients
├─ messages_app/            # Email templates/messages
├─ mailings/                # Campaigns, logs, attempts
│  ├─ services.py           # Sending logic with audit & logging
│  └─ management/commands/  # Scheduler & CLI commands
├─ users/                   # Custom user model
├─ templates/               # Django templates
├─ static/                  # Static assets
├─ logs/                    # Application & scheduler logs
├─ scripts/                 # PowerShell / Batch helpers
├─ docker-compose.yml
├─ docker-compose.prod.yml
├─ Dockerfile
└─ manage.py
⚙️ Key features
✉️ Email campaigns
Campaign lifecycle (draft → scheduled → sent)

Manual and scheduled sending

Dry-run mode for testing

🧵 Background processing
Celery workers for sending emails

Redis as broker and cache

django-apscheduler for periodic tasks

📊 Reporting & audit
Per-recipient delivery logs

Aggregated mailing attempts

Statuses: SENT / ERROR / DRY_RUN

🧠 Caching
Redis-based server cache

HTTP client caching (Cache-Control, Last-Modified)

Optimized reports with cached querysets

🔐 Access control
Custom user model (email as login)

Ownership-based permissions

Manager roles with extended visibility

🚀 Quick start (local)
1️⃣ Install dependencies
Poetry (recommended)

poetry env use 3.13
poetry install
or venv + pip

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
2️⃣ Environment variables
Create .env in project root:

DEBUG=True
SECRET_KEY=replace-me
ALLOWED_HOSTS=localhost,127.0.0.1

DB_NAME=message_autosend
DB_USER=postgres
DB_PASSWORD=password
DB_HOST=127.0.0.1
DB_PORT=5432

REDIS_URL=redis://127.0.0.1:6379/1

EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USE_TLS=True
SMTP_USER=example@gmail.com
SMTP_PASSWORD=app_password
DEFAULT_FROM_EMAIL=robot@example.com
3️⃣ Database setup
python manage.py migrate
python manage.py createsuperuser
Optional demo data:

python manage.py seed_demo
python manage.py seed_managers
4️⃣ Run server
python manage.py runserver
Open: http://127.0.0.1:8000

🐳 Docker (production-like)
docker compose -f docker-compose.prod.yml up -d --build
Services:

Django (Gunicorn)

PostgreSQL

Redis

Celery worker

Celery beat

Nginx

🧪 Useful commands
python manage.py run_scheduler
python manage.py send_due_mailings
python manage.py send_mailing --id 123 --dry-run
python manage.py showmigrations
📎 Why this project matters
This repository demonstrates:

real backend architecture (not a tutorial app)

async & scheduled processing

Redis usage beyond “just cache”

clean separation of concerns

production-oriented setup with Docker

Ideal as a portfolio project for backend Python/Django roles.

👤 Author
Alex Scherbyna
Backend Python / Django developer

GitHub: https://github.com/ScherbAlex
Логи «заняты»
Значит уже запущен планировщик или другой процесс пишет в тот же лог. Остановите через stop_scheduler.ps1/.bat.Продолжайте писать — аудитория растёт!
