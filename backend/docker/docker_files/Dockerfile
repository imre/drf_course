# Use the arm64 variant of the python:3.10 image
FROM --platform=linux/arm64 python:3.10-slim-bullseye

# Set environment variables
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

# Install OS-level dependencies
# build-essential contains compilers (gcc) needed for many Python packages
# libpq-dev is for compiling psycopg2 (PostgreSQL driver), a common Django dependency
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    netcat-openbsd \
    gettext \
    # Clean up apt cache to reduce image size
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /code

# Upgrade pip and install Python requirements
# Using --no-cache-dir reduces final image size
COPY ./requirements.txt .
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Ensure the entrypoint script is executable
RUN chmod +x /code/docker/entrypoints/entrypoint.sh

EXPOSE 8000

# Set the entrypoint for the container
ENTRYPOINT ["/code/docker/entrypoints/entrypoint.sh"]
