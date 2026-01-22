## Beginner Project 02 – Containerized Infrastructure

### Preamble

> Containers are the Pandora’s box of tech—open them, and you unleash endless dependencies.
Docker keeps them all contained, but one typo in the YAML, and you’ve summoned digital chaos.
Docker Compose is great—until you realize it’s herding cats with a bash script.
Debugging a container is like doing surgery through a keyhole while blindfolded.
But without containers, your deployments would be a Frankenstein of patchwork servers and shattered dreams.

<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project02-Containerization/imgs/compose.png" width="500">
</p>

---

## Introduction

This project aims to broaden your system administration and DevOps skills by having you design and run a **multi-service infrastructure** using Docker and Docker Compose.

You will:

- Build several Docker images yourself (no pre-built service images).
- Orchestrate them with Docker Compose inside a virtual machine.
- Expose a single HTTPS entrypoint via NGINX on your personal domain `FirstnameLastname.um6p.ma`.
- Run a Python web application stack (framework of your choice) backed by MariaDB.
- Optionally add a WordPress stack alongside your Python app, under the same infrastructure constraints.

This project is meant to be a serious step up in difficulty after Projects 00 (Virtualization) and 01 (Provisioning).

---

## Learning Goals

By the end of this project, you should be able to:

- Design a small, realistic multi-container infrastructure.
- Write robust Dockerfiles for multiple services (web, database, reverse proxy).
- Use Docker Compose to build, network, and run services together.
- Use named volumes to persist data and understand where it lives on the host.
- Configure services using environment variables and `.env` files without leaking secrets.
- Terminate TLS in NGINX and route traffic securely to internal services.
- Compare and reason about Virtual Machines vs Docker, secrets vs environment variables, Docker network vs host network, and volumes vs bind mounts.

---

## General Guidelines

- This project **must** be done inside a virtual machine.
- All files required to configure your infrastructure must live in a `srcs/` directory at the root of your repository.
	- A typical structure could be: `srcs/docker-compose.yml`, `srcs/.env`, and per-service folders such as `srcs/nginx/`, `srcs/app/`, `srcs/mariadb/` (each containing its own `Dockerfile` and configuration files).
- A `Makefile` at the root of your repository is **required**. It must:
  - Build your images using `srcs/docker-compose.yml`.
  - Provide at least the following targets (or equivalents):
	 - `make up` – build and start the full stack.
	 - `make down` – stop and remove containers (but keep volumes).
	 - `make clean` – remove containers, networks, and volumes (this will delete your persistent data volumes).
- Each service must run in its own container; **one service per container**.
- Each service must have its **own Dockerfile**. Docker images must be built from these Dockerfiles via Docker Compose (no `docker run` hacks).
- For performance and size, containers must be built from a recent, explicit stable version of Alpine or Debian (no `latest`), using pinned tags (for example, `alpine:3.19`, `debian:12`).
- Using pre-built service images (NGINX, MariaDB, Python app images, WordPress, etc.) is **forbidden**. Only base OS images are allowed.
- The Docker image name must match its corresponding service name.
- The `latest` tag is **prohibited**.
- Using `network: host`, `--link` or `links:` is **forbidden**.
- Your containers must not be kept alive by hacky infinite loops or fake daemons (e.g., `tail -f`, `sleep infinity`, `while true`, or `bash` as PID 1). Design proper entrypoints and processes.
- Do not store passwords, API keys, or secrets in your Dockerfiles or in your Git repository. It is **mandatory** to use environment variables and `.env`.

---

## Mandatory Infrastructure

You must set up, using Docker Compose, a small infrastructure composed of **different services under strict rules**. All services must run inside your virtual machine.

### Services Overview

You must configure at least the following services:

1. **NGINX (reverse proxy)**
	- Runs only NGINX (no PHP, no application code).
	- Exposes port **443** on the host.
	- Terminates TLS using **TLSv1.2 or TLSv1.3 only**.
	- Acts as the **only entrypoint** to your infrastructure.
	- Reverse-proxies HTTPS requests from `FirstnameLastname.um6p.ma` to your internal app services over the Docker network.

2. **Python Application Service (mandatory)**
	- Runs a Python web application using a framework of your choice (e.g., Django, Flask, FastAPI).
	- Uses a proper application server (e.g., `gunicorn`, `uvicorn`, or similar). No NGINX inside this container.
	- Listens on an **internal port only** (e.g., 8000) and is reachable only from NGINX and other internal services via the Docker network.
	- Reads its configuration (database host, user, password, etc.) from environment variables.

3. **MariaDB Service (mandatory)**
	- Runs **only MariaDB** (no web server, no application logic).
	- All configuration is driven by environment variables.
	- Stores its data in a dedicated **named volume** (see Volumes section below).
	- Must contain at least one database and appropriate users for your Python app (and optionally WordPress).

### Volumes and Data Persistence

You must set up **named volumes** for persistent data storage:

- A named volume that contains your MariaDB database data.
- A second named volume that contains your application data (for example, uploaded media or static assets built by your Python framework).
- Both named volumes must store their data under `/home/<username>/data` on the host machine. Replace `<username>` with your own username.
- **Bind mounts are not allowed** for these persistent volumes.

### Networks

- Create at least one dedicated Docker network that connects your containers (NGINX, Python app, MariaDB, and any optional services).
- All inter-service communication must go through this Docker network.
- Your services must use **service names** (as defined in `docker-compose.yml`) to reach each other, not IP addresses.

### Container Restart Policies

- Your containers must be configured with restart policies so they automatically restart in case of crashes.

### Domain Name & TLS

- You must configure your host machine so that `FirstnameLastname.um6p.ma` resolves to the IP address of your virtual machine (for example, via `/etc/hosts`).
- Your NGINX container must be the **only entrypoint** to your infrastructure via port **443**.
- No other container may publish ports to the host.
- NGINX must serve your site over HTTPS using **TLSv1.2** or **TLSv1.3**.
- You may use a self-signed certificate generated specifically for this project; you must document how you generated it and how you configured NGINX to use it. Using Let’s Encrypt or another CA is allowed but not required.

---

## Choosing Your Python Framework

You must implement your main application using a Python web framework. You may choose among:

- Django
- Flask
- FastAPI
- Or another modern Python web framework agreed upon by your instructors.

Constraints for your Python app container:

- Runs as a non-root user inside the container wherever reasonably possible.
- Uses an explicit Alpine or Debian base image with a pinned tag (no `latest`).
- Uses a proper process as PID 1 (e.g., `gunicorn`, `uvicorn`), not a debug shell or infinite loop.
- Does **not** use development servers such as `flask run` or `python manage.py runserver` in the production container; you must use a proper WSGI/ASGI server (e.g., `gunicorn`, `uvicorn`).
- Does **not** expose itself directly to the host; it only listens on an internal port and is reachable via NGINX.
- Reads configuration (DB host, user, password, secrets) exclusively from environment variables and/or configuration files populated from env vars.

You must document which framework you chose and justify your choice briefly in your project README.

---

## Optional: WordPress Service

In addition to your Python app, you may add a **WordPress + php-fpm** service to your infrastructure as an advanced/bonus feature:

- WordPress must run in its own container with **php-fpm only** (no NGINX).
- It must have its own Dockerfile (no pre-built WordPress image).
- It must connect to the same MariaDB service (with its own database/schema and users).
- WordPress files must be stored in a dedicated named volume under `/home/<username>/data` on the host.
- NGINX must reverse-proxy to WordPress via the Docker network (for example, by routing `/blog` or a separate hostname to the WordPress container).

>This WordPress service is **optional** and should only be attempted after your Python stack is fully functional. It will only be evaluated if the mandatory part is perfect.

---

## Environment Variables, .env, and Secrets

- You must use environment variables to configure your services (database credentials, domain name, etc.).
- A `.env` file must be present in `srcs/` to store non-secret configuration values (for example, `DOMAIN_NAME=FirstnameLastname.um6p.ma`, DB usernames, and database names).
- Database usernames and database names are **not** considered secrets in this project; passwords, tokens, and API keys **are** secrets.
- **No passwords, API keys, or confidential secrets may be committed to Git**. Any such data must be stored locally (for example, in files listed in `.gitignore` or via Docker secrets).
- Any credentials, API keys, or passwords found in your Git repository (outside of properly configured secrets) should be considered a project failure and may cause your project to be refused for evaluation.

---

## Testing Your Infrastructure

Before considering your project complete, you should validate it thoroughly:

1. **Build and Run**
	- Use your `Makefile` to build and start the whole stack (e.g., `make up`).
	- Ensure that all containers start without errors.

2. **Domain & HTTPS**
	- Confirm that `FirstnameLastname.um6p.ma` resolves to your VM’s IP.
	- Access `https://FirstnameLastname.um6p.ma` in a browser and verify that you reach your Python app via NGINX over TLS.

3. **Application & Database**
	- Verify that your Python app can connect to MariaDB using service names and environment variables.
	- Confirm that data written by your app persists across `docker compose down` / `up` cycles (with volumes preserved).

4. **Optional WordPress** (if implemented)
	- Verify that WordPress is reachable through NGINX (for example, at `/blog`).
	- Confirm it uses MariaDB and its own named volume for files.

5. **Resilience & Restart Policies**
	- Stop one of the containers and ensure it restarts according to your restart policy.

6. **Clean Up & Rebuild**
	- Use your `Makefile` to stop and clean up (`make down` / `make clean`).
	- Rebuild and start the entire stack from scratch and verify that it behaves the same.

Document in your README which tests you performed and any issues you encountered.

---

## Documentation Requirements

At a minimum, your repository should contain:

- A `README.md` at the root of your repository that includes:
  - An italicized first line indicating authorship (for example: *This project has been created as part of the Club Cloud curriculum by Firstname Lastname*).
  - A **Description** section that clearly presents the project, its goal, and a brief overview of the stack.
  - An **Instructions** section describing how to build, run, stop, and clean the project using your `Makefile` and Docker Compose.
  - A **Resources** section listing key documentation (Docker, Docker Compose, NGINX, framework docs, MariaDB, TLS) and a brief note on how AI tools were used (for which tasks and which parts of the project).
  - A **Project Description & Design Choices** section that explains your overall design and includes short comparisons between:
	 - Virtual Machines vs Docker
	 - Secrets vs Environment Variables
	 - Docker Network vs Host Network
	 - Docker Volumes vs Bind Mounts

For higher difficulty and better practice, you are encouraged (or you may be required by your instructor) to also provide:

- `USER_DOC.md` – User/administrator documentation explaining:
  - What services are provided.
  - How to start and stop the stack.
  - How to access the website and any admin interfaces.
  - How to locate and manage credentials.
  - How to check that services are running correctly.

- `DEV_DOC.md` – Developer documentation describing:
  - How to set up the environment from scratch (prerequisites, configuration files, secrets handling).
  - How to build and launch the project using the `Makefile` and Docker Compose.
  - Useful commands to manage containers and volumes.
  - Where data is stored and how it persists.

---

## Deliverables

Use this section as a checklist when you think you are done.

### Structure & Tooling

- [ ] A `srcs/` directory containing all configuration files, `docker-compose.yml`, `.env`, and per-service folders.
- [ ] A root `Makefile` that can build, start, stop, and clean the entire stack.
- [ ] One Dockerfile per service (NGINX, Python app, MariaDB, optional WordPress), each using Alpine or Debian with explicit, non-`latest` tags.

### Mandatory Stack Behavior

- [ ] NGINX is the only entrypoint, listening on port 443, serving `FirstnameLastname.um6p.ma` over TLSv1.2/1.3.
- [ ] The Python app is reachable only through NGINX (no direct exposure to the host), runs via an appropriate app server, and communicates with MariaDB over the Docker network.
- [ ] MariaDB runs in its own container with proper initialization and persistent data stored in a named volume.
- [ ] Named volumes for DB and app data are configured and store data under `/home/<username>/data` on the host; no bind mounts are used for these volumes.
- [ ] A dedicated Docker network connects all services; services use service names, not IPs.
- [ ] Containers have restart policies and restart on crashes.

### Configuration, Secrets & .env

- [ ] Environment variables are used to configure services (DB credentials, domain name, etc.).
- [ ] A `.env` file exists in `srcs/` for non-secret configuration values.
- [ ] No passwords, API keys, or secrets are committed to Git or hard-coded in Dockerfiles.

### Documentation

- [ ] `README.md` is present, written in English, and contains all required sections (Description, Instructions, Resources including AI usage, Project Description & Design Choices with comparisons).
- [ ] (If required) `USER_DOC.md` explains how to use and operate the stack.
- [ ] (If required) `DEV_DOC.md` explains how to develop, build, and maintain the stack.

### Optional WordPress (if implemented)

- [ ] WordPress + php-fpm runs in its own container with its own Dockerfile.
- [ ] WordPress uses MariaDB via the Docker network and its own DB credentials from environment variables.
- [ ] WordPress files are stored in a dedicated named volume under `/home/<username>/data`.
- [ ] NGINX reverse-proxies to WordPress (for example, via a separate path or hostname) over HTTPS on port 443.

If all the mandatory checks are satisfied and your stack behaves as expected, you are ready for peer review and evaluation.

<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project02-Containerization/imgs/container-meme.png" width="500">
</p>

