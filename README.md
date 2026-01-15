# DevOps Lab 8 — Docker Compose (Flask+Redis, Prometheus+Grafana)

**Демидов Матвей Александрович, ФИТ-1-2024 НМ**  
**Дисциплина:** Методы и инструменты DevOps  
**Лабораторная работа:** ЛР по лекции 8 (Docker Compose)

---

## 1) Задание
Собрать 2 Docker Compose стенда:

1) **Flask + Redis**  
- веб-приложение (страница со счётчиком посещений)  
- сохранение счётчика в Redis  
- endpoint `/metrics` для метрик

2) **Prometheus + Grafana (+ blackbox_exporter)**  
- Prometheus собирает метрики приложения и делает probe URL  
- Grafana подключена к Prometheus и доступна через веб-интерфейс

---

## 2) Стенд и доступ
Работа выполнялась на **VM2 (Docker host)**: `10.0.2.16`  
SSH из Windows: `ssh student@127.0.0.1 -p 2223`

Порты (VirtualBox port forwarding):
- Flask: `http://127.0.0.1:8000`
- Prometheus: `http://127.0.0.1:9090`
- Grafana: `http://127.0.0.1:3000`

---

## 3) Структура репозитория
- `flask_redis/` — compose + исходники Flask приложения
- `monitoring/` — compose + конфиги Prometheus/Grafana/blackbox_exporter
- `screenshots/` — скриншоты выполнения
- `logs/` — сохранённые логи команд (ps, up, curl и т.п.)

---

## 4) Часть 1 — Flask + Redis

### 4.1 Запуск (VM2)
```bash
cd ~/DevOpsLab8/flask_redis
docker compose up -d --build
docker compose ps
```

### 4.2 Проверка
```bash
curl -s http://127.0.0.1:8000
curl -s http://127.0.0.1:8000/metrics | head -n 20
```

---

## 5) Часть 2 — Prometheus + Grafana (+ blackbox)

### 5.1 Запуск (VM2)
```bash
cd ~/DevOpsLab8/monitoring
docker compose up -d
docker compose ps
```

### 5.2 Проверка Prometheus Targets
В Prometheus открыть: `Status → Targets`  
Должны быть job'ы (UP):
- `prometheus`
- `flask_metrics`
- `blackbox-http`

### 5.3 Вход в Grafana
`http://127.0.0.1:3000`  
Логин: `admin`  
Пароль: `grafana`  

---

## 6) Скриншоты

### 6.1 Flask+Redis: контейнеры подняты
![](screenshots/01_flask_compose_ps.png)

### 6.2 Flask+Redis: страница в браузере (счётчик)
![](screenshots/02_flask_browser.png)

### 6.3 Flask+Redis: /metrics
![](screenshots/03_flask_metrics.png)

### 6.4 Monitoring: контейнеры подняты
![](screenshots/04_monitoring_compose_ps.png)

### 6.5 Prometheus: Targets UP
![](screenshots/05_prom_targets.png)

### 6.6 Grafana: интерфейс после входа
![](screenshots/06_grafana_ok.png)
