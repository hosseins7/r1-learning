# Dockerfile located in app directory
FROM python:3.10-slim-buster

# Set work directory
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

# Install netcat
RUN apt-get install -y netcat

# Copy the entire application code into the Docker image
COPY ./app /usr/src/app

# Copy manage.py explicitly
COPY ./app/manage.py /usr/src/app/manage.py

# Copy docker-entrypoint.sh and ensure it's in Unix format
COPY ./app/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN dos2unix /usr/local/bin/docker-entrypoint.sh

# Make entrypoint executable
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Set entrypoint
ENTRYPOINT ["bash", "/usr/local/bin/docker-entrypoint.sh"]
