# AHON FloodWatch

AHON FloodWatch is a flood monitoring and early-warning system for Barangay Tonsuya, Malabon. It combines water-level sensor readings, an AI-assisted forecast, a web dashboard, and configurable SMS alerts to help residents and barangay staff monitor flood conditions and respond to rising water levels.

---

## Group Members & Roles

* **Padillo, Jenny Rose O.** – Project Manager / Team Leader
* **Almario, Trisha Nicole V.** – Documentation
* **Vargas Jr., Victorino** – Debugging and Testing
* **Jimenez, Don Chester A.** – UI/UX Developer

---

## Tech Stack

* **Frontend:** React, Vite, Material UI
* **Backend:** Python, Django, Django REST Framework
* **Database:** PostgreSQL (Docker); Firebase Realtime Database integration
* **AI:** Python, scikit-learn
* **Hardware and alerts:** Arduino water-level sensor, SIM900/SIM800 GSM modem
* **Tools:** Docker Compose, Git, VS Code, Arduino IDE

---

## System Requirements & Prerequisites

For the recommended Docker setup, install:

* Git
* Docker Desktop with Docker Compose

For local development without Docker, you will also need:

* Python 3.9 or later
* Node.js 18 or later and npm
* PostgreSQL 13 or later

Arduino IDE and compatible sensor/GSM hardware are only required for hardware-based readings and SMS alerts. Firebase and SMS credentials are needed to enable their corresponding integrations.

---

## Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/teakaiii/AhonFloodWatch-GROUP1-BYAHENGAPAT.git
cd AhonFloodWatch-GROUP1-BYAHENGAPAT
```

### 2. Configure environment files

In the repository root, copy the example files:

```powershell
Copy-Item docker\.env.example docker\.env
Copy-Item backend\Django\.env.example backend\Django\.env
```

Edit both files before starting the services:

* In `docker/.env`, set a strong `DB_PASSWORD` and a private Django `SECRET_KEY`. Configure Firebase, SMS, and AI values if you plan to use those integrations.
* In `backend/Django/.env`, set `DB_HOST=postgres` and make `DB_NAME`, `DB_USER`, and `DB_PASSWORD` match the values in `docker/.env`. Set a private `SECRET_KEY` and the appropriate `ALLOWED_HOSTS`.
* Keep environment files and service-account credentials private; do not commit them.

The example environment files contain placeholders and should be configured before production use.

### 3. Build and start the application

From the repository root, run:

```powershell
Set-Location docker
docker-compose up -d --build
docker-compose exec backend python manage.py migrate
docker-compose exec backend python manage.py createsuperuser
```

The PostgreSQL container creates the configured database automatically, and Django migrations create its tables. No separate SQL dump is required. The database design is documented in [database/DATABASE_SCHEMA.md](database/DATABASE_SCHEMA.md).

### 4. Open the application

* **Dashboard:** http://localhost
* **Backend API:** http://localhost:8000/api
* **API documentation:** http://localhost:8000/swagger/
* **Django admin:** http://localhost:8000/admin

To stop the services, run `docker-compose down` from the `docker` directory. To remove database data as well, use `docker-compose down -v`; this permanently deletes the database volume.

---

## Local Development

Detailed instructions for running the Django backend, React frontend, and AI module separately are available in the [Quick Start Guide](docs/QUICK_START_GUIDE.md) and [Developer Guide](docs/DEVELOPER_GUIDE.md). Backend dependencies are listed in `backend/Django/requirements.txt`; frontend dependencies and commands are in `frontend/React/package.json`.

Useful checks:

```powershell
# Backend tests
Set-Location backend\Django
python manage.py test

# Frontend production build
Set-Location ..\..\frontend\React
npm install
npm run build
```

---

## Documentation

* [Quick Start Guide](docs/QUICK_START_GUIDE.md)
* [Developer Guide](docs/DEVELOPER_GUIDE.md)
* [User Manual](docs/USER_MANUAL.md)
* [API Documentation](docs/API_DOCUMENTATION.md)
* [Hardware Setup](docs/HARDWARE_SETUP.md)
* [Deployment Guide](docs/DEPLOYMENT_GUIDE.md)
* [Security Guide](docs/SECURITY_GUIDE.md)
* [System Architecture](docs/SYSTEM_ARCHITECTURE.md)
* [Database Schema](database/DATABASE_SCHEMA.md)
* [ISO/IEC 25010:2023 Evaluation Plan](docs/ISO_25010_2023_EVALUATION.md)

---

## Default Login Credentials

There are no shared default credentials. Create an administrator during setup with `python manage.py createsuperuser`, then sign in at http://localhost:8000/admin.

---

## License

This project is intended for educational and community use.
