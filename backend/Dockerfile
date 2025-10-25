# ----------------------------
# Stage 1: Build Environment
# ----------------------------
FROM python:3.11-slim AS builder

# Prevent Python from writing .pyc files and buffer logs
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Copy dependency file first to leverage Docker caching
COPY requirements.txt .

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential gcc curl && \
    pip install --upgrade pip && \
    pip install -r requirements.txt && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# ----------------------------
# Stage 2: Runtime Environment
# ----------------------------
FROM python:3.11-slim

WORKDIR /app

# Copy installed packages from builder
COPY --from=builder /usr/local /usr/local

# Copy project files
COPY . .

# Expose internal container port 8080 (not 8000)
EXPOSE 8080

# Run FastAPI on port 8080
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]