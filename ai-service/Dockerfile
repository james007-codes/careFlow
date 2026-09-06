FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV UV_PROJECT_ENVIRONMENT=/opt/venv

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

COPY pyproject.toml uv.lock ./

# Install the exact CPU-only PyTorch build.
RUN uv venv /opt/venv && \
    uv pip install \
    --python /opt/venv/bin/python \
    --index-url https://download.pytorch.org/whl/cpu \
    torch==2.14.0

# Install the remaining locked dependencies,
# while leaving the CPU-only torch installation untouched.
RUN uv sync --frozen --no-install-package torch

COPY app ./app
COPY sample_docs ./sample_docs

# Build the general FAISS knowledge base.
RUN uv run --no-sync python -m app.retrieval.ingest

EXPOSE 8080

CMD ["sh", "-c", "uv run --no-sync uvicorn app.main:app --host 0.0.0.0 --port ${PORT:-8080}"]