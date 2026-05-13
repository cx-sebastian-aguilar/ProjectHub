FROM python:3.11-slim

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Set environment variables
ENV DATABASE_URL="postgresql://projecthub:password123@db:5432/projecthub"
ENV JWT_SECRET="secret_key_12345"
ENV AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
ENV AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
ENV UV_SYSTEM_PYTHON=1 \
    UV_NO_CACHE=1

EXPOSE 5000 8000 9000

# Install system dependencies
RUN apt-get update && apt-get install -y \
    postgresql-client \
    libpq-dev \
    netcat-openbsd \
    dos2unix \
    gcc \
    g++ \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy project definition and install Python dependencies
COPY pyproject.toml .
RUN uv sync --no-dev

# Copy application code
COPY . .

# Fix line endings and permissions
RUN dos2unix /app/*.sh 2>/dev/null || true && \
    dos2unix /app/docker-entrypoint.sh && \
    chmod +x /app/docker-entrypoint.sh && \
    mkdir -p /app/uploads /app/logs && \
    chmod 777 /app/uploads && \
    chmod 777 /app/logs

# Use bash explicitly to handle volume mount permission issues
CMD ["/bin/bash", "/app/docker-entrypoint.sh"]

