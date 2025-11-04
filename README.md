# SOA Tourism App 

A service‑oriented, microservices‑based tourism platform developed as part of a university SOA course. The system covers tour management, user interaction (blogs, comments, likes, following), purchases, and tour execution (with a position simulator). The backend is fully containerized via **Docker Compose**, fronted by a **Traefik** gateway, and instrumented with a complete **observability** stack (Prometheus, Grafana, Loki, Jaeger, cAdvisor, Node Exporter).

---

## Key Features

- **Microservices** - independently deployable services, each with its own datastore.
- **Gateway (Traefik)** - central entrypoint, routing, service discovery via labels.
- **Communication** - REST, RPC (**gRPC**), and async pub/sub (**NATS**) where applicable.
- **Observability** - Prometheus (metrics), Grafana (dashboards), Loki (logs), Jaeger (tracing), cAdvisor/Node Exporter (host & container metrics).
- **Docker‑first** - reproducible builds, isolated networks, named volumes, and a single `docker compose` to run the stack.
- **Security & Roles** - Admin, Guide, Tourist (authorization/role checks at endpoints).

---

## Services (implemented / planned)

- **Auth Service** - registration, authentication, roles (Admin, Guide, Tourist).
- **Blog Service** - blog posts, comments, likes (Markdown support).
- **Tour Service** - tours, geo key points, statuses (draft/published/archived), distance calculation, position simulation & execution tracking.
- **Followers Service** - follow graph & recommendations (**Neo4j**).
- **Purchase Service** - cart, order items, checkout / purchase tokens.
- **Gateway** - **Traefik** reverse proxy with per‑service routing.
- **Monitoring Stack** - Prometheus, Grafana, Loki (+ Promtail), Jaeger, cAdvisor, Node Exporter.

> Some services may be stubs or partially implemented in this iteration.

---

## Tech Stack 

- **Platform:** Docker, Docker Compose
- **Gateway/Proxy:** Traefik
- **Datastores:** MongoDB (documents), Neo4j (graph)
- **Messaging:** NATS (pub/sub)
- **RPC:** gRPC
- **Observability:** Prometheus, Grafana, Loki, Promtail, Jaeger, cAdvisor, Node Exporter
- **Testing:** Postman / cURL

---

## Getting Started

### Prerequisites

- Docker & Docker Compose installed
- (Optional) Ensure the ports below are available on your machine

### Quick Run

```bash
docker compose up --build
# or in background:
docker compose up -d --build
```

### Default Local Endpoints

- **Gateway (API entry):** `http://localhost:8080`
- **Grafana:** `http://localhost:3000`
- **Prometheus:** `http://localhost:9090`
- **Jaeger:** `http://localhost:16686`
- **Loki:** `http://localhost:3100`

> Swagger/OpenAPI UIs are exposed per service (check each service’s README or `docker-compose.yml` labels/ports).

---

## Docker & Gateway Notes

- Each service has a dedicated **Dockerfile**; `docker-compose.yml` builds, networks, and runs everything together.
- **Traefik** is configured via labels on services in `docker-compose.yml` for automatic routing and discovery.
- **Named volumes** persist database data (MongoDB, Neo4j) and observability components as needed.
- A dedicated **bridge network** enables secure, container‑internal service communication.

---

## Observability

- **Metrics** - Services expose metrics endpoints consumed by **Prometheus**. **cAdvisor** and **Node Exporter** provide container and host metrics.
- **Dashboards** - **Grafana** visualizes application and infra metrics (CPU, memory, FS, network, error rates, latency, RPS).
- **Logs** - **Promtail** ships container logs to **Loki**; you can query and correlate logs in Grafana Explore.
- **Tracing** - **Jaeger** collects distributed traces across REST/RPC requests; use it to analyze latency and bottlenecks.

---

## Development

- Run individual services locally or through Docker. Prefer Docker for parity with the full stack.
- Configure environment variables via `.env` and service‑specific configs. **Do not commit real secrets.**
- Use Postman / cURL for endpoint testing; keep shared collections in `/postman` (if present).
- When adding new services, provide:
  - `Dockerfile`
  - Traefik labels (router rule, service name, port)
  - Health/metrics endpoints
  - `docker-compose.yml` service block

---

## Authors
<a href="https://github.com/bgdj11">
  <img src="https://github.com/bgdj11.png?size=80" width="48" height="48" alt="@bgdj11" />
</a>
<a href="https://github.com/milosmat">
  <img src="https://github.com/milosmat.png?size=80" width="48" height="48" alt="@milosmat" />
</a>
