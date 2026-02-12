# Use the cleanstart python base image
FROM cleanstart/python:latest-dev

# Set working directory
WORKDIR /app

USER root

RUN apk update && apk search pip

# Create non-root user for security
RUN adduser --disabled-password --gecos '' appuser && \
    chown -R appuser:appuser /app

RUN python -m venv /venv

ENV PATH="/venv/bin:$PATH"

# Copy requirements first for better caching
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Create directory for database with proper permissions
RUN mkdir -p /app/data && \
    chown -R appuser:appuser /app/data

# Switch to non-root user
USER appuser

# Expose port
EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:5000/health || exit 1

# Set environment variables
ENV DATABASE_PATH=/app/data/users.db
ENV DEBUG=false
ENV HOST=0.0.0.0
ENV PORT=5000

# Run the application
ENTRYPOINT ["python"]
CMD ["app.py"]