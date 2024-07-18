# Dockerfile located in app directory
FROM python:3.10-slim-buster

# Set work directory
COPY . /usr/src/app/
WORKDIR /usr/src/app

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install system dependencies
RUN apt-get update \
    && apt-get install -y gcc default-libmysqlclient-dev dos2unix \
    && apt-get clean

# Upgrade pip and install dependencies
RUN pip install --upgrade pip setuptools wheel

# Copy requirements.txt and install dependencies
COPY ./app/requirements.txt .
RUN pip install -r requirements.txt --no-cache-dir

# Copy docker-entrypoint.sh and ensure it's in Unix format
COPY ./app/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN dos2unix /usr/local/bin/docker-entrypoint.sh

# Make entrypoint executable
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Copy the rest of the application code
COPY . .

# Install netcat
RUN apt-get install netcat -y

# Define entrypoint
ENTRYPOINT ["bash", "/usr/local/bin/docker-entrypoint.sh"]
