# Microservices Profiling for a Social Network on DeathStarBench

This project involves the deployment and performance analysis of a social network application built on a microservices architecture (DeathStarBench). We have extended the original benchmark by integrating a comprehensive monitoring stack, including **Prometheus, Grafana, Jaeger, cAdvisor, and the Blackbox Exporter**, to provide deep observability into the system's performance.

This project was completed by:
* [cite_start]Suraj Yadav (MT24098) [cite: 4]
* [cite_start]Vishal Shriram Bhosle (MT24104) [cite: 4]
* [cite_start]Komal Parmar (MT24047) [cite: 4]
* [cite_start]Siddharth Variya (MT24091) [cite: 4]

---

## 🎯 Project Goals

The primary goals of this project were to:

* [cite_start]**Deploy:** Successfully deploy the entire social network application, with all its microservices running in Docker containers. [cite: 10]
* [cite_start]**Load Testing:** Simulate a realistic read/write workload using `wrk2` to stress-test the application. [cite: 11]
* [cite_start]**Tracing & Monitoring:** Instrument the services with Jaeger for distributed tracing and use Prometheus and Grafana for metrics collection and visualization. [cite: 12]
* [cite_start]**Bottleneck Identification:** Analyze the collected trace data and performance metrics to identify performance hotspots and sources of latency within the microservices architecture. [cite: 13]

---

## 🏗️ System Architecture

[cite_start]The application is deployed using Docker Compose, with services communicating via Thrift RPC calls. [cite: 16, 17]

### Monitoring Stack

We integrated a robust monitoring stack to gain full observability over the application:

| Component         | Purpose                                         | Port  |
| ----------------- | ----------------------------------------------- | ----- |
| **Prometheus** | Metrics collection, storage, and querying.      | 9090  |
| **Grafana** | For creating and visualizing interactive dashboards. | 3000  |
| **Jaeger** | Distributed tracing and transaction monitoring. | 16686 |
| **cAdvisor** | Monitors container resource usage (CPU, memory). | 8081  |
| **Blackbox Exporter** | Probes external endpoints for health checks.    | 9115  |

[cite_start]*Table based on information from the project presentation. [cite: 146]*

---

## 🛠️ Customizations and Implementation

This implementation includes several key additions to the standard DeathStarBench setup:

* [cite_start]**Additional Services:** The `docker-compose.yml` file was updated to include services for `Prometheus`, `Grafana`, `cAdvisor`, and `Blackbox Exporter`. [cite: 150]
* [cite_start]**Health Checks:** Robust health checks were added for each service in the Docker Compose file to ensure a stable startup order and resilience. [cite: 151]
* **Prometheus Configuration (`prometheus.yml`):** Configured to scrape metrics from:
    * [cite_start]Itself (`prometheus`) [cite: 155]
    * [cite_start]Jaeger (`jaeger-agent`) [cite: 156]
    * [cite_start]cAdvisor (`cadvisor`) [cite: 158]
    * [cite_start]The social network services via the Blackbox Exporter. [cite: 160]

---

## 🚀 Getting Started

Follow these steps to get the entire application stack running on your local machine.

### Prerequisites

* **Docker:** [Install Docker](https://docs.docker.com/get-docker/)
* **Docker Compose:** [Install Docker Compose](https://docs.docker.com/compose/install/) (usually included with Docker Desktop)

### Running the Application

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```

2.  **Start the Services:**
    Launch all services in detached mode using Docker Compose.
    ```bash
    docker-compose up -d
    ```
    It may take several minutes for all services to start and become healthy.

### Accessing the Services

Once the stack is running, you can access the various UIs:

* **Media Frontend**: `http://localhost:8082`
* [cite_start]**Grafana Dashboard**: `http://localhost:3000` (Login with `admin`/`admin` or anonymous access) [cite: 20]
* [cite_start]**Jaeger UI**: `http://localhost:16686` [cite: 146]
* [cite_start]**Prometheus UI**: `http://localhost:9090` [cite: 146]
* [cite_start]**cAdvisor**: `http://localhost:8081` [cite: 146]
* **Nginx-Thrift**: `http://localhost:8080`

---

## 📊 Evaluation and Insights

[cite_start]We conducted load testing using **wrk2** with 12 threads, 400 connections, and a constant rate of 10 requests per second for 300 seconds. [cite: 162]

### Key Observations

* [cite_start]**Bottlenecks Identified:** The analysis of Jaeger traces and Grafana dashboards revealed that the `user-timeline-service` and `post-storage-service` are the primary bottlenecks, especially under write-heavy loads. [cite: 165, 177]
* [cite_start]**Read vs. Write Performance:** Read-heavy operations, such as reading a timeline, scaled well and showed low latency. [cite: 173] [cite_start]Write-heavy operations, like composing a post, exhibited significant delays and saturation points, indicating a need for optimization. [cite: 166, 179]
* [cite_start]**System Health:** Prometheus and Blackbox monitoring showed that all services remained available during the tests, although latency spiked for certain services under load. [cite: 168, 170]

---

## 🔮 Future Work

Based on our findings, we recommend the following next steps:

* [cite_start]**Database Optimization:** Focus on optimizing the database queries and schema for the `user-timeline` and `post-storage` services to improve write performance. [cite: 185]
* [cite_start]**Increased Load Testing:** Perform further stress tests with a higher number of connections (e.g., 500-600) to understand the system's breaking points. [cite: 186]
* [cite_start]**Tracing Overhead:** Tune Jaeger's sampling strategy and retention policies to reduce the performance overhead of distributed tracing in a production environment. [cite: 187]
