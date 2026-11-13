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

The following commands are for **Windows PowerShell**. Open a separate PowerShell terminal for each service, and run them from the repository root unless a command changes directory. Local Django development uses SQLite by default; Docker deployment uses PostgreSQL.

### 1. Prepare the backend environment (once)

```powershell
cd "C:\Users\Admin\Desktop\AHON-FloodWatch-3\backend\Django"
python -m venv venv
.\venv\Scripts\python.exe -m pip install -r requirements.txt
.\venv\Scripts\python.exe manage.py migrate
```

### 2. Start the Django backend

In the first PowerShell terminal:

```powershell
cd "C:\Users\Admin\Desktop\AHON-FloodWatch-3\backend\Django"
.\venv\Scripts\python.exe manage.py runserver 8000
```

### 3. Start the React frontend

In a second PowerShell terminal:

```powershell
cd "C:\Users\Admin\Desktop\AHON-FloodWatch-3\frontend\React"
npm.cmd install
npm.cmd run dev
```

Open the Vite URL printed in the terminal (normally http://localhost:5173).

### 4. Optional: start the Arduino serial bridge

Connect and configure the Arduino, then run this in a third PowerShell terminal. Set credentials for a Django account that is authorized to submit readings; enter your own password rather than putting it in a command or this public README.

```powershell
cd "C:\Users\Admin\Desktop\AHON-FloodWatch-3"
$env:FLOODWATCH_USER = "your-django-username"
$env:FLOODWATCH_PASSWORD = Read-Host "Django account password"
.\.venv\Scripts\python.exe .\scripts\serial_bridge.py --port COM8 --gsm-status connected
```

The bridge requires `pyserial` and `requests` in the root `.venv`. If they are not installed, install them with:

```powershell
.\.venv\Scripts\python.exe -m pip install pyserial requests
```

For hardware-free testing, omit `--port COM8` and use `--simulate`; simulation does not send SMS. See the [Hardware Setup Guide](docs/HARDWARE_SETUP.md) for wiring and sensor configuration.

Backend dependencies are listed in `backend/Django/requirements.txt`; frontend commands and dependencies are in `frontend/React/package.json`. More setup details are available in the [Quick Start Guide](docs/QUICK_START_GUIDE.md) and [Developer Guide](docs/DEVELOPER_GUIDE.md).

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
