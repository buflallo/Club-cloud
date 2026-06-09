# Beginner Project 06 - Delivery and Observability Fundamentals

## Introduction

You can now build infrastructure. The next step is learning how software changes move safely through that infrastructure, and how operators know whether the system is healthy.

This project introduces release engineering and observability on a low-end setup. You will build a small application, package it, test it, release it through a pipeline, and expose enough runtime signals to debug it without guessing.

The goal is not to create a perfect production platform. The goal is to understand the fundamentals behind CI/CD, health checks, logs, metrics, traces, dashboards, alerts, and rollback thinking.

This project must make release boundaries explicit. Building an image is not deployment. Copying files manually is not CD. Rebuilding a broken system is not rollback.

---

## Learning Goals

By the end of this project, you should be able to:

- Explain the difference between CI and CD.
- Design a pipeline with test, build, package, and deploy stages.
- Use pipeline secrets without committing credentials.
- Promote a version using immutable image tags.
- Add meaningful health checks to an application.
- Produce logs, metrics, and traces from application behavior.
- Define a small SLI/SLO and connect it to an alert.
- Explain how rollback differs from rebuild, redeploy, and destroy.

---

## Mandatory System

You must build or reuse a small web application with at least two services:

- one public API or frontend service
- one internal dependency, such as a second API, worker, cache, or database

The system must run locally using Docker Compose or the infrastructure from Project 05.

Your system must include:

- A health endpoint.
- Structured logs or consistent log format.
- Basic application metrics.
- At least one distributed trace or trace-like request correlation across service boundaries.
- A dashboard showing service health.
- At least one alert rule tied to a user-facing symptom.
- A visible version endpoint or version label showing which release is running.

---

## Pipeline Requirements

Your repository must include a CI/CD workflow using GitHub Actions or an equivalent pipeline tool approved by instructors.

The pipeline must include:

- Lint or static validation.
- Automated tests.
- Docker image build.
- Image tagging that does not rely on `latest`.
- Image push to either a public registry, private registry, or local registry.
- Secret handling through pipeline secret storage.
- A deploy or release stage targeting your local/lab environment.
- A failure path that prevents promotion.

Manual approval gates are optional at this level, but you must explain where one would belong.

If you use GitHub-hosted runners, they cannot directly deploy to a laptop or private VM unless you expose a reachable target. For a low-end local setup, use one of these patterns and document the tradeoff:

- CI runs in GitHub Actions, release/deploy runs locally from the tagged image.
- CI runs in GitHub Actions, deploy uses a self-hosted runner in the lab.
- CI runs in another instructor-approved tool that can reach the lab environment.

---

## Observability Requirements

Your runtime stack must collect enough signals to answer:

- Is the service up?
- Is it serving requests successfully?
- Is it slow?
- Which dependency failed?
- Which version is currently running?
- What changed before the failure?

You may choose the tools. Common choices include:

- Prometheus for metrics.
- Grafana for dashboards.
- Loki or plain container logs for logs.
- OpenTelemetry with Jaeger or Tempo for traces.

Tool choice matters less than whether the signals are useful.

---

## Rollback and Failure Requirements

You must demonstrate one controlled failure.

Examples:

- broken image tag
- failing health endpoint
- bad configuration value
- dependency outage
- high error rate

You must document:

- how the failure appeared in logs, metrics, traces, or dashboard
- whether the pipeline caught it
- how you restored service
- what rollback means in your design
- which artifact was reused during rollback

---

## Constraints

- Keep the project runnable on a normal student laptop.
- Do not require paid cloud services.
- Do not commit secrets, tokens, `.env` files with passwords, or generated credentials.
- Do not use `latest` tags.
- Do not rebuild an old image and call it rollback; rollback must reuse a known previous artifact.
- Do not make dashboards that only prove tools are installed; dashboards must answer operational questions.
- Do not call a system observable if it only prints logs.

---

## Documentation Requirements

Your README must explain:

- Application architecture.
- Pipeline stages.
- Image tagging strategy.
- Registry strategy.
- Secret handling approach.
- Health check behavior.
- Metrics, logs, and tracing choices.
- SLI/SLO definition.
- Alert rule and why it matters.
- Failure test and recovery steps.

Include screenshots or copied outputs only when they prove behavior. Avoid filling the README with tool installation notes.

---

## Evaluation Focus

Be ready to explain:

- What each pipeline stage protects against.
- What artifact is promoted.
- Why your health check is meaningful.
- Which dashboard panel you would look at first during an incident.
- How traces or correlation IDs help follow one request.
- How your rollback works and what it cannot fix.
- Why your chosen deploy method is or is not real CD.

---

## Suggested Research Areas

- GitHub Actions workflow syntax, runners, artifacts, environments, and secrets.
- Docker image tagging and registries.
- Health checks, readiness, liveness, and deployment gates.
- Prometheus metrics and PromQL basics.
- OpenTelemetry concepts: traces, spans, context propagation.
- SLOs, SLIs, error budgets, and alert fatigue.
