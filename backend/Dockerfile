FROM python:3.10-slim

ENV PYTHONUNBUFFERED=1

# Install uv
COPY --from=ghcr.io/astral-sh/uv:0.9.26 /uv /uvx /bin/

WORKDIR /app/

# Copy package requirements configuration normally
COPY ./pyproject.toml /app/

# Install the dependencies directly into the container system layer
RUN uv pip install --system -r pyproject.toml

# Copy the actual app components safely
COPY ./scripts /app/backend/scripts
COPY ./pyproject.toml ./alembic.ini /app/backend/
COPY ./app /app/backend/app

WORKDIR /app/backend/

CMD ["fastapi", "run", "--workers", "4", "app/main.py"]
