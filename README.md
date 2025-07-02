
-----

### **Paste the following content into your main `README.md` file:**

````markdown
# DeathStarBench - Social Network Profiling and Monitoring Setup

This document provides step-by-step instructions for deploying the DeathStarBench social network application, which has been integrated with a comprehensive monitoring stack for performance analysis and bottleneck identification.

## Overview

This project centers on implementing and evaluating a microservices-based social network platform. The system enables key functionalities such as user registration, creating posts, and viewing timelines[cite: 891]. The core of this project is a detailed performance assessment using Jaeger for distributed tracing, `wrk2` for workload simulation, and a full monitoring stack including Prometheus and Grafana[cite: 892].

## Project Goal

Social networks demand highly scalable and fault-tolerant architectures[cite: 894]. This project's primary objective is to gain hands-on experience in profiling and deploying microservices to address these challenges[cite: 896].

The specific goals are:
* **Deploy:** Start and run all social-network microservices within Docker containers[cite: 60].
* **Load Testing:** Run a realistic read/write workload with `wrk2` to test the system's performance under different conditions[cite: 61, 899].
* **Tracing & Monitoring:** Use Jaeger for distributed tracing and a full monitoring stack to observe system behavior and collect metrics[cite: 62, 912].
* **Bottleneck Identification:** Analyze trace and performance data to find hotspots, latency sources, and recommend optimizations[cite: 63, 900].

## Architecture Overview

The Social Network is composed of multiple microservices that communicate using **Thrift RPCs**[cite: 907].

### Application Services
* **User Service**: Handles user authentication and management.
* **Post Storage Service**: Manages user posts.
* **Timeline Services**: Generates home and user timelines.
* **Social Graph Service**: Maintains follower/following relationships.
* **Supporting Services**: `media-service`, `text-service`, `url-shorten-service`, etc.
* **Databases**: MongoDB and Redis.

### Integrated Monitoring Stack
To facilitate end-to-end monitoring, the following tools were incorporated[cite: 908]:
* **Prometheus**: A time-series database for collecting metrics from all services[cite: 909].
* **Grafana**: A visualization tool for creating interactive dashboards from Prometheus data[cite: 910].
* **cAdvisor**: Monitors container resource utilization (CPU, memory, network I/O)[cite: 911].
* **Jaeger**: Pre-integrated for distributed tracing to visualize request flows and identify latency bottlenecks[cite: 912].
* **Blackbox Exporter**: Included to monitor external service endpoints using TCP probes to ensure availability[cite: 913, 930].

## Setup Instructions

### 1. Prerequisites

Ensure you have the following installed on a Linux-based system:
* Docker & Docker Compose
* Python 3
* `git` and `make`

```bash
# Install required system dependencies
sudo apt update && sudo apt install -y python3-pip luajit luasocket libssl-dev make
````

### 2\. Clone the Repository

```bash
# Clone your repository (replace with your URL) and its submodules
git clone --recurse-submodules [https://github.com/Komal682/DeathStarBench_SocialNetwork_Project.git](https://github.com/Komal682/DeathStarBench_SocialNetwork_Project.git)
cd DeathStarBench_SocialNetwork_Project/
```

### 3\. Build the Workload Generator

The `wrk2` tool must be built before use. This is a one-time step.

```bash
cd wrk2
make
cd ..
```

### 4\. Start Services Using Docker Compose

Navigate to the `socialNetwork` directory and launch all services.

```bash
cd socialNetwork
docker compose up -d
```

You can check the status of the running containers with `docker ps`. It may take a few minutes for all services to become healthy.

### 5\. Initialize the Social Graph

Run the following script to populate the database with test users and their social connections.

```bash
python3 scripts/init_social_graph.py --graph=socfb-Reed98
```

*If you encounter a `ModuleNotFoundError`, create a virtual environment and install dependencies:*

```bash
# python3 -m venv venv
# source venv/bin/activate
# pip install aiohttp
```

## Running Performance Tests

[cite\_start]The following commands simulate the four different workloads used in our evaluation[cite: 2, 3]. [cite\_start]These tests were configured to run with **12 threads**, **400 concurrent connections** for a **duration of 300 seconds**, at a rate of **10 requests per second**[cite: 938, 941].

All test commands should be run from the `socialNetwork` directory.

#### Read Home Timeline

```bash
../wrk2/wrk -D exp -t 12 -c 400 -d 300s -L -s ../wrk2/scripts/social-network/read-home-timeline.lua http://localhost:8080/wrk2-api/home-timeline/read -R 10
```

#### Read User Timeline

```bash
../wrk2/wrk -D exp -t 12 -c 400 -d 300s -L -s ../wrk2/scripts/social-network/read-user-timeline.lua http://localhost:8080/wrk2-api/user-timeline/read -R 10
```

#### Compose Post

```bash
../wrk2/wrk -D exp -t 12 -c 400 -d 300s -L -s ../wrk2/scripts/social-network/compose-post.lua http://localhost:8080/wrk2-api/post/compose -R 10
```

#### Mixed Workload

```bash
../wrk2/wrk -D exp -t 12 -c 400 -d 300s -L -s ../wrk2/scripts/social-network/mixed-workload.lua http://localhost:8080/wrk2-api/mixed -R 10
```

## Accessing the Monitoring Dashboards

Once the services are running, you can access the various UIs from your web browser:

  * **Grafana Dashboard**: `http://localhost:3000`
  * [cite\_start]**Jaeger UI (Tracing)**: `http://localhost:16686` [cite: 916]
  * [cite\_start]**Prometheus UI**: `http://localhost:9090` [cite: 916]
  * [cite\_start]**cAdvisor (Container Stats)**: `http://localhost:8081` [cite: 916]

## Stopping and Cleaning Up

To stop all running services:

```bash
docker compose down
```

To stop services and remove all data volumes:

```bash
docker compose down -v
```

## Additional Documentation

For more detailed information, please refer to the following documents in this repository:

  * **[Architecture Deep Dive](https://www.google.com/search?q=docs/ARCHITECTURE.md)**: Understand the system design and the integrated monitoring stack.
  * **[Setup and Usage Guide](https://www.google.com/search?q=docs/SETUP.md)**: Detailed step-by-step instructions to deploy and test the application.
  * **[Evaluation and Results](https://www.google.com/search?q=docs/EVALUATION.md)**: A complete analysis of the performance tests and bottleneck identification.

<!-- end list -->

```
```