# VotingApp - Containerized Spring Boot Application
<img width="1536" height="1024" alt="docker architectural image" src="https://github.com/user-attachments/assets/81895360-7788-4ca6-9f0d-331898498a7b" />

This project demonstrates the containerization of a **VotingApp** built with **Spring Boot Java**, using **Docker** and **Docker Compose**. The application includes multiple services: the VotingApp backend, MySQL database, and PHPMyAdmin for database management, orchestrated with Docker Compose.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Project Structure](#project-structure)
4. [Docker Setup](#docker-setup)
5. [Running Services](#running-services)
6. [Testing the Application](#testing-the-application)
7. [Errors Encountered and How I Solved Them](#errors-encountered-and-how-i-solved-them)
8. [Cleanup](#cleanup)
9. Pushing image to Docker hub
10. [Notes](#notes)

---

## Project Overview

The VotingApp allows users to participate in polls and track voting results. The project leverages containerization to simplify deployment and ensure consistency across environments.

* **Backend:** Spring Boot Java application
* **Database:** MySQL
* **Database Management:** PHPMyAdmin
* **Containerization:** Docker + Docker Compose
* **Networking:** Docker network for service communication
* **Persistent Storage:** Docker volumes

---

## Prerequisites

Before running the project, ensure the following are installed:

* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Java JDK 17](https://adoptium.net/) (for building Spring Boot locally)
* [Maven](https://maven.apache.org/install.html) (for building the Spring Boot app, optional if using prebuilt Docker image)
* [MYSQL8] 
* AWS EC2 instance (for deployment and testing)

---

## Project Structure

```
VotingApp/
├── Dockerfile
├── docker-compose.yml
├── src/
│   └── main/java/...  # Spring Boot application source
├── pom.xml
└── README.md
```

* **Dockerfile:** Builds the VotingApp Docker image.
* **docker-compose.yml:** Defines all services, networks, and volumes.
* **src/**: Application source code.
* **pom.xml:** Maven dependencies.

---

## Docker Setup

### Build the Docker Image

```bash
docker build -t votingapp-image:v1 .
```

### Docker Compose Services

The `docker-compose.yml` includes:

* **votingapp:** Spring Boot application
* **mysql-db:** MySQL database
* **phpmyadmin:** PHPMyAdmin interface
* **volumes:** Persist MySQL data
* **network:** Connects all services

---

## Running Services

### Start Services

```bash
docker compose up -d
```

### Check Running Containers

```bash
docker ps
```

### Access Services

* VotingApp: `http://localhost:8080`
* PHPMyAdmin: `http://localhost:8081`

  * Username: root
  * Password: rootpassword (or as defined in docker-compose)

---

## Testing the Application

### Locally

1. Start Docker Compose: `docker compose up -d`
2. Verify services are running: `docker ps`
3. Test backend(votingapp) using curl:

```bash
curl -i http://localhost:8080
```

4. Access database via PHPMyAdmin to verify tables and data.

### On EC2

1. Launch EC2 instance and install Docker & Docker Compose.
2. Copy project files to EC2.
3. Build and start services on EC2:

```bash
docker build -t votingapp-image:v1 .
docker compose up -d
```

4. Ensure security group allows ports `8080` and `8081`.
5. Test endpoints via EC2 public IP:

```bash
curl http://<EC2_PUBLIC_IP>:8080/api/votes
```

---

## Errors Encountered and How I Solved Them

### 1. `no space left on device` Error

* **Problem:** While the running container, the EC2 instance ran out of disk space.
* **Troubleshooting Steps:**

  1. Checked disk usage:

  ```bash
  df -h
  ```

  2. Listed Docker images and containers consuming space:

  ```bash
  docker images
  docker ps -a
  docker system df
  ```

  3. Removed unused Docker containers, images, volumes:

  ```bash
  docker system prune -a -f
  docker volume prune -f
  ```

  4. Verified free space and restarted Docker Compose services.
* **Solution:** Freed space before adding extra volumes to EC2.
* EC2 instance-storage-volume-modify volume. 25Gib was added 
* This ensured Docker containers could run without hitting storage limits.

---

## Cleanup

Stop and remove containers, networks, and volumes:

```bash
docker compose down -v
docker system prune -f
```


Pushing Images to Docker Hub
1. Login to Docker Hub
docker login
2. Tag the image using your Docker Hub username

My username: edidiongakpan18

docker tag votingapp-image:v1 edidiongakpan18/votingapp-image:v1
3. Push the image
docker push edidiongakpan18/votingapp-image:v1
4. Same steps was repeated for subsequent images
5. Verify the image is available on Docker Hub

Visit:
https://hub.docker.com/u/edidiongakpan18

---

## Notes

* Persistent storage is handled with Docker volumes to keep MySQL data intact across container restarts.
* Docker network ensures isolation and secure communication between services.
* This setup can be extended for production deployments with additional configurations.
* Troubleshooting storage errors early prevents runtime failures on EC2.


THIS README-DOCKER.md that contains the Docker containerization project was written by me @Edidiongakpan18 while the main README.md file was written by the developer of the springboot application
credit: Github/swapnilbamble1438
