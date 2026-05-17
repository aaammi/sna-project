# System Status and Uptime Monitor

A self-hosted web service monitoring system that periodically checks the availability of websites, measures response times, stores results in a database, and visualizes the data in a Grafana dashboard.

## Features

- Automated availability checks for multiple URLs (every 60 seconds)
- Response time measurement and status code tracking
- PostgreSQL storage for historical check data
- Grafana dashboard with uptime and response time graphs
- NGINX as a reverse proxy for the dashboard
- Full Docker Compose setup — runs with a single command
- GitHub Actions CI/CD pipeline for automated linting and build checks

## Tech Stack

| Component | Role |
|---|---|
| Python | Worker script that pings websites |
| PostgreSQL | Stores check history |
| Grafana | Visualizes uptime and response time |
| NGINX | Reverse proxy for Grafana |
| Docker / Docker Compose | Containerization |
| GitHub Actions | CI/CD pipeline |

## Project Structure

```
.
├── worker/
│   ├── uptime_worker.py        # Main worker script
│   ├── db_utils.py             # Database utilities
│   ├── init_db.sql             # Database schema
│   ├── requirements.txt
│   └── Dockerfile
├── nginx/
│   └── nginx.conf              # Reverse proxy config
├── grafana/
│   └── provisioning/
│       └── datasources/
│           └── postgres.yml    # Auto-configured datasource
├── docker-compose.yml          # Main compose file
├── docker-compose.test.yml     # Test compose file
└── .github/
    └── workflows/
        └── main.yml            # CI/CD pipeline
```

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Run locally

1. Clone the repository:
```bash
git clone https://github.com/aaammi/sna-project.git
cd sna-project
```

2. Create the environment file:
```bash
cp worker/env.example .env
```

3. Start all services:
```bash
docker compose up -d
```

4. Open Grafana at `http://localhost:8080`
   - Login: `admin` / `admin`
   - PostgreSQL datasource is pre-configured automatically

### Configuration

Edit `.env` to change monitored URLs or intervals:

```env
URLS=https://google.com,https://github.com,http://example.com
CHECK_INTERVAL_SECONDS=60
REQUEST_TIMEOUT=5
```

## Monitored URLs (default)

| URL | 
|---|
| https://google.com |
| https://github.com |
| http://example.com |

Can be changed in `.env` file via `URLS` variable.

## Database Schema

```sql
CREATE TABLE checks (
    id                SERIAL PRIMARY KEY,
    url               TEXT NOT NULL,
    timestamp         TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    response_time_ms  INTEGER,
    status_code       INTEGER,
    is_success        BOOLEAN NOT NULL DEFAULT FALSE,
    error_message     TEXT
);
```

## Contributors

| Name | Role |
|---|---|
| Ekaterina Ivanova | CI/CD, GitHub Actions, Grafana dashboards |
| Aminat Dzhamalova | Infrastructure, Docker Compose, NGINX, Grafana setup |
| Dzhamilia Zinnurova | Backend, Python worker, PostgreSQL |
