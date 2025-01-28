# Use a base image with Python
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy the application code into the container
COPY ./src/app2 /app

# Install dependencies
RUN pip install -r requirements.txt

# Command to run the app
CMD ["python", "main.py"]
