FROM python:3.10

ENV PYTHONUNBUFFERED=1

# Install uv
COPY --from=ghcr.io/astral-sh/uv:0.9.26 /uv /uvx /bin/

# Compile bytecode
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

WORKDIR /app/

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

# Copy package requirements configuration normally
COPY ./pyproject.toml /app/

# Generate the missing lockfile and install dependencies natively
RUN uv pip install -r pyproject.toml

# Copy the actual app components safely
COPY ./scripts /app/backend/scripts
COPY ./pyproject.toml ./alembic.ini /app/backend/
COPY ./app /app/backend/app

WORKDIR /app/backend/

CMD ["fastapi", "run", "--workers", "4", "app/main.py"]
