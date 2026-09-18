# 🚀 Dockerized Logistic Regression MLOps Microservice

A compact, educational MLOps repository demonstrating model training, Flask REST API serving, and production Docker containerization.

---

## 📁 Project Structure

```
.
├── train.py              # Model training (Logistic Regression -> artifacts/model.pkl)
├── app.py                # Flask REST API server (Port 5001)
├── run_model.py          # CLI prediction tool
├── requirements.txt      # Python dependencies
├── Dockerfile            # Optimized container build instructions
└── docker-compose.yml    # Single-command orchestration configuration
```

---

## 💡 Key Architectural & Learning Decisions

### 1. 📦 Base Image: `python:3.11-slim`
- **Why**: Standard `python:3.11` is `~1 GB`. The `-slim` variant is **~150 MB** (80% smaller).
- **Benefit**: Faster image builds, quicker network pulls/pushes, and reduced attack surface.

### 2. ⚡ Environment Variables: `ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1`
- **`PYTHONUNBUFFERED=1`**: Forces Python output directly to standard output (`stdout`) without buffering, enabling real-time `docker logs` streaming.
- **`PYTHONDONTWRITEBYTECODE=1`**: Prevents Python from writing `.pyc` compiled bytecode files inside the container.

### 3. 🚀 Layer Caching (`COPY requirements.txt .` before `COPY . .`)
- **Why**: Docker caches layers sequentially.
- **Benefit**: When you edit `app.py`, Docker reuses cached `pip install` layers instead of re-downloading dependencies, reducing rebuild time to seconds.

### 4. 🧹 Storage Optimization: `pip install --no-cache-dir`
- **Why**: Prevents `pip` from storing cached `.whl` installation files inside the container layer.

### 5. 🔌 Port Exposure: `EXPOSE 5001`
- **Why**: `app.py` runs Flask on port `5001`. `EXPOSE` documents the port mapping for container networks.

### 6. 🎼 Single-Command Orchestration: `docker-compose.yml`
- **Why**: Replaces complex `docker run -d -p 5001:5001 --name ...` terminal commands with a single configuration file defining builds, ports, container names, and restart policies.

---

## 🚀 Quick Start

### 1. Start Application with Docker Compose
```bash
docker compose up -d --build
```

### 2. Test Endpoints

- **Health Check**:
  ```bash
  curl http://localhost:5001/health
  ```

- **Prediction Request**:
  ```bash
  curl -X POST "http://localhost:5001/predict" \
       -H "Content-Type: application/json" \
       -d "{\"features\":[5.1, 3.5, 1.4, 0.2]}"
  ```

### 3. Stop Application
```bash
docker compose down
```
