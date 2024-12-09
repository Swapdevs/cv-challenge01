# Use the latest official Python image as a base
FROM python:latest

# Install system dependencies (including Rust for pydantic-core)
RUN apt-get update && apt-get install -y \
    nodejs \
    npm \
    build-essential \
    libpq-dev \
    rustc \
    cargo \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry using pip (ensures Poetry is installed for the Python environment)
RUN pip install --no-cache-dir pydantic-core==2.16.3 --use-pep517 && install poetry

# Set the working directory
WORKDIR /app

# Copy only the dependency-related files first to leverage Docker cache
COPY pyproject.toml poetry.lock /app/

# Install project dependencies using Poetry
RUN poetry install --no-dev --no-root

# Copy the rest of the application files
COPY . .

# Ensure the prestart script is executable
RUN chmod +x ./prestart.sh

# Expose the port FastAPI runs on
EXPOSE 8000

# Run the prestart script and start the FastAPI server using Uvicorn
CMD ["sh", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload"]

