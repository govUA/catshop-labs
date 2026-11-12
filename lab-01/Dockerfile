# --- Stage 1: Builder ---
FROM python:3.12-slim AS builder

WORKDIR /build
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# --- Stage 2: Production Runtime ---
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PORT=8080

RUN groupadd --system appgroup && \
    useradd --system --gid appgroup --create-home appuser && \
    mkdir -p /data && \
    chown -R appuser:appgroup /data && \
    python -m pip uninstall -y pip setuptools wheel

WORKDIR /app

COPY --from=builder /install /usr/local
COPY --chown=appuser:appgroup app.py .

USER appuser

EXPOSE 8080

CMD ["python", "app.py"]