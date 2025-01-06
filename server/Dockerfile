FROM python:3.12.0-slim-bookworm

# Set environment variables
ENV PYTHONBUFFERED=1 \
    PYTHONWRITEBYTECODE=1 \
    APP=/app

# Change the working directory
WORKDIR $APP

# Install system dependencies (optional but often necessary for Python projects)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt $APP/
RUN pip3 install --no-cache-dir -r requirements.txt

# Copy the application files
COPY . $APP/

# Expose the port for the application
EXPOSE 8000

# Ensure the entrypoint script is executable
RUN chmod +x /app/entrypoint.sh

# Define the entrypoint
ENTRYPOINT ["/bin/bash", "/app/entrypoint.sh"]

# Set the default command to run Gunicorn
CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "djangoproj.wsgi"]
