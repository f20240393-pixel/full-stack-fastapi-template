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

# Copy configuration files normally to the build environment
COPY ./pyproject.toml ./uv.lock /app/

# Standard dependency installation without strict --mount constraints
RUN uv sync --frozen --no-install-workspace --package app

# Copy code modules safely
COPY ./scripts /app/backend/scripts
COPY ./pyproject.toml ./alembic.ini /app/backend/
COPY ./app /app/backend/app

# Finalize workspace sync
RUN uv sync --frozen --package app

WORKDIR /app/backend/

CMD ["fastapi", "run", "--workers", "4", "app/main.py"]
