# Debezium PostgreSQL to Kafka Integration with Docker Compose

This project sets up a data pipeline to capture and stream changes from a PostgreSQL database to Kafka using Debezium, an open-source CDC (Change Data Capture) tool. The pipeline is containerized using Docker Compose, and it includes all necessary components to manage and monitor the streaming process.

## Project overview
The setup includes the following services:
- PostgreSQL: The source database for capturing changes.
- Zookeeper: Coordinates distributed processes and manages Kafka configuration.
- Kafka: Streams changes captured by Debezium from the PostgreSQL database.
- Debezium: Connects to PostgreSQL, captures changes, and pushes them to Kafka topics.
- Kafka UI: Web-based interface to view and manage Kafka topics.
- Debezium UI: Web-based interface for configuring and managing Debezium connectors.
- Init-Connect: Initializes connectors for Debezium based on custom configurations in the /connector directory.

## Prerequisites
- `Docker` and `Docker Compose` must be installed.
- Ports `5432`, `2181`, `9092`, `8083`, `8084`, and `8080` must be available on your local machine.

## Getting started
1. Clone the repository:
```bash
git clone https://github.com/yourusername/debezium-pg-kafka.git
cd debezium-pg-kafka
```

2. Prepare connector configuration
Place your custom Debezium connector configuration file (JSON format) in the `/connector` directory. This configuration tells Debezium how to connect to the PostgreSQL database and which tables to monitor.

3. Start the docker compose services
Run the following command to bring up all services:
```bash
docker-compose up -d
```
This command will pull the necessary images, create containers, and start all services in detached mode.

4. Verify service health
You can verify if all services are running by checking Docker logs or visiting the UI interfaces:

- Kafka UI: [http://localhost:8084](http://localhost:8084)
- Debezium UI: [http://localhost:8080](http://localhost:8080)


## Services Overview

### postgres
- **Port**: `5432`
- **Description**: A PostgreSQL database with Change Data Capture (CDC) enabled to track changes.
- **Credentials**:
  - **Username**: `postgres`
  - **Password**: `postgres`

### zookeeper
- **Port**: `2181`
- **Description**: Zookeeper manages kafka brokers and configurations, ensuring smooth coordination between distributed processes.

### kafka
- **Port**: `9092`
- **Description**: Kafka serves as the message broker that receives change events from debezium and allows consumers to read those events from topics.

### debezium
- **Port**: `8083`
- **Description**: Debezium Connect instance that listens for changes in postgres and publishes them to kafka topics.

### kafka-ui
- **Port**: `8084`
- **URL**: [http://localhost:8084](http://localhost:8084)
- **Description**: A web-based interface for managing kafka clusters, viewing topics, partitions, and message contents.

### debezium-ui
- **Port**: `8080`
- **URL**: [http://localhost:8080](http://localhost:8080)
- **Description**: A web-based interface for configuring and managing debezium connectors, viewing real-time connector status, and troubleshooting.

### init-connect
- **Description**: This service initializes and sets up debezium connectors automatically, based on custom configurations located in the `/connector` directory. It starts once all dependencies (debezium, kafka, and postgres) are running.

## Networks: 
All services are on a shared network `dbz-net` to allow inter-service communication.
## Volumes: 
The `/connector` directory is mounted in `init-connect` for loading connector configurations.