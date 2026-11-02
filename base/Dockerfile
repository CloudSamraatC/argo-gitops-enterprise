# Use a lightweight Python image
FROM python:3.12-slim

# Set working directory
WORKDIR /app

# Copy application
COPY app.py .

# Application listens on port 8080
EXPOSE 8080

# Run the application
CMD ["python", "app.py"]
