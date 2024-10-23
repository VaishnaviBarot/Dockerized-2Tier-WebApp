# Dockerized 2-Tier Web Application deployment to Amazon-ECR

## Description

This repository contains the setup and deployment instructions for a 2-tier web application using Docker and MySQL. The application is designed to run locally and integrates a MySQL database with a Python-based web application. 

Additionally, the repository incorporates a Continuous Integration and Continuous Deployment (CI/CD) workflow using GitHub Actions. This automation simplifies the build and deployment process, allowing the application and its MySQL database images to be built and pushed to Amazon Elastic Container Registry (ECR) automatically whenever changes are made to the main branch.

### Features
- **MySQL Integration**: Easily set up and configure a MySQL database using Docker.
- **Dockerized Application**: Build and run the web application as a Docker container, simplifying the development environment setup.
- **Environment Variable Configuration**: Flexible configuration using environment variables for database connectivity and application settings.
- **Local Development**: Step-by-step instructions to run the application and database locally for testing and development.
- **Automated CI/CD**: Utilize GitHub Actions to automate the building and deployment of Docker images to Amazon ECR, ensuring seamless updates and consistency in the deployment process.

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

### CI/CD Workflow

This repository uses GitHub Actions for continuous integration and deployment (CI/CD). The workflow is defined in the `.github/workflows/deploy.yml` file and includes the following steps:

1. **Check out Code**: Pulls the latest code from the repository.
2. **Login to Amazon ECR**: Authenticates with Amazon ECR using AWS credentials stored in GitHub secrets.
3. **Install Dependencies**: Updates `pip` and installs the required Python dependencies.
4. **Build and Push Main Application Image**: Builds the Docker image for the main application and pushes it to ECR.
5. **Build and Push MySQL Image**: Builds the Docker image for MySQL and pushes it to ECR.

### Prerequisites
- Docker installed on your machine.
- Python and required libraries as specified in the `requirements.txt` file.
- An AWS account with permissions to use Amazon ECR.
- Amazon ECR repositories created for both the application and MySQL images.
- GitHub repository secrets configured with your AWS credentials:
  - `AWS_ACCESS_KEY_ID`
  - `AWS_SECRET_ACCESS_KEY`
  - `AWS_SESSION_TOKEN`

### Usage
Follow the instructions in this repository to set up the application locally, and make sure it is accessible via your web browser. For automated deployments to AWS, push changes to the `main` branch to trigger the GitHub Actions workflow.
