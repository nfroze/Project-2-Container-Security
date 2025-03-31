# Use the official Python 3.9 Alpine image, which is minimal and secure
FROM python:3.9-alpine

# Set the working directory inside the container
WORKDIR /app

# Install necessary build dependencies and clean up unnecessary files
# This ensures the image gets the latest security patches
RUN apk update && apk upgrade && apk add --no-cache \
    build-base \
    && rm -rf /var/cache/apk/*

# Install the latest version of setuptools to fix vulnerabilities
RUN pip install --upgrade setuptools

# Copy the contents of the local "app" directory to the container's /app directory
COPY app /app

# Expose port 80 so the container can communicate with the outside world
EXPOSE 80

# Run Python's built-in HTTP server on port 80 to serve the app
CMD ["python3", "-m", "http.server", "80"]