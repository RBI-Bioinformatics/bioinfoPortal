# Dockerized Drupal Project

This repository contains a Dockerized Drupal project designed to make it easy to set up, develop, and deploy a Drupal website. Follow the steps below to set up and run the project on another device.

## Prerequisites

Before you start, ensure that the following dependencies are installed on your system:

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Git**: [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- **Composer**: [Install Composer](https://getcomposer.org/download/)

## Getting Started

### 1. Clone the Repository

Start by cloning the repository to your local machine:

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

### 2. Set Up Environment Variables

If an `.env` file is provided, make a copy of the example file and configure it with your environment variables:

```bash
cp .env.example .env
```

### 3. Install Composer Dependencies

Navigate to the Drupal project directory and install the necessary dependencies using Composer:

```bash
cd docroot/tripal4
composer install
```

### 4. Build and Start Docker Containers

Build and start the Docker containers using Docker Compose:

```bash
docker-compose up --build -d
```
