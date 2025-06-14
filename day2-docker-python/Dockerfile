# Use lightweight official Python image
FROM python:3.9-alpine

# Create group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Install build dependencies needed for some Python packages
RUN apk add --no-cache gcc musl-dev libffi-dev

# Copy dependency files first for caching
COPY requirements.txt .

# Install Python dependencies as non-root user
RUN pip install --upgrade pip setuptools && \
    pip install --no-cache-dir -r requirements.txt

# Copy app source code
COPY app.py .

# Fix ownership for non-root user
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose the Flask default port
EXPOSE 5000

# Healthcheck endpoint to verify container is healthy
HEALTHCHECK --interval=30s --timeout=5s CMD curl -f http://localhost:5000/health || exit 1

# Command to run the app
CMD ["python", "app.py"]
