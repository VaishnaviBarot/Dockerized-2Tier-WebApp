# 2-Tier Web Application with Docker and MySQL

## Description

This repository contains the setup and deployment instructions for a 2-tier web application using Docker and MySQL. The application is designed to run locally and integrates a MySQL database with a Python-based web application.

### Features
- **MySQL Integration**: Easily set up and configure a MySQL database using Docker.
- **Dockerized Application**: Build and run the web application as a Docker container, simplifying the development environment setup.
- **Environment Variable Configuration**: Flexible configuration using environment variables for database connectivity and application settings.
- **Local Development**: Step-by-step instructions to run the application and database locally for testing and development.

### Getting Started
1. Install the required MySQL package:
    ```bash
    sudo apt-get update -y
    sudo apt-get install mysql-client -y
    ```
2. Build the Docker images for both the MySQL database and the web application:
    - **Building MySQL Docker image**:
      ```bash
      docker build -t my_db -f Dockerfile_mysql .
      ```
    - **Building application Docker image**:
      ```bash
      docker build -t my_app -f Dockerfile .
      ```
3. Run the MySQL container:
    ```bash
    docker run -d -e MYSQL_ROOT_PASSWORD=pw my_db
    ```
4. Get the IP of the database and export it as the `DBHOST` variable:
    ```bash
    docker inspect <container_id>
    ```
    Example when running DB as a Docker container and app running locally:
    ```bash
    export DBHOST=127.0.0.1
    export DBPORT=3307
    ```
    Example when running DB as a Docker container and app running in Docker:
    ```bash
    export DBHOST=172.17.0.2
    export DBPORT=3306
    ```
5. Set up other environment variables:
    ```bash
    export DBUSER=root
    export DATABASE=employees
    export DBPWD=pw
    export APP_COLOR=blue
    ```
6. Run the application and ensure it is visible in the browser:
    ```bash
    docker run -p 8080:8080 -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e DBUSER=$DBUSER -e DBPWD=$DBPWD my_app
    ```

### Prerequisites
- Docker installed on your machine.
- Python and required libraries as specified in the `requirements.txt` file.

### Usage
Follow the instructions in this repository to set up the application locally, and make sure it is accessible via your web browser.





