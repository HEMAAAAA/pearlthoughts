
# E-commerce Platform with Docker Compose

This project provides a fully containerized e-commerce platform using Docker Compose. It includes multiple microservices, each responsible for a specific part of the platform, such as user authentication, product management, order processing, and more.

## Prerequisites

- Docker installed on your system.
- Docker Compose installed (if not, follow the [official guide](https://docs.docker.com/compose/install/)).

## Deploy Your Application with Docker Compose
    
2.  Clone the repository containing the `docker-compose.yml` file.

1.  Build each image by following the documentation for each service, which will show you how to build the images.

3.  Navigate to the directory where the `docker-compose.yml` file is located.
    
4.  Run the following command to start all services:


   ```bash
   docker-compose up -d
   ```
This will start all the microservices and their associated databases in detached mode.

5.  Access the application via `http://ec2-public-ip:4000>`

