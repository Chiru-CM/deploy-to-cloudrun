# Flask App Deployment on GCP Cloud Run

Containerized Python Flask application deployed to GCP Cloud Run via automated GitHub Actions CI/CD pipeline.

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python / Flask | Web application |
| Docker | Containerization |
| Google Artifact Registry | Container image storage |
| GCP Cloud Run | Serverless container deployment |
| GitHub Actions | CI/CD pipeline |

## CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/deploy.yml`) triggers on every push to `main` and runs the following steps:

| Step | Action |
|------|--------|
| Checkout | Pull latest code |
| GCP Auth | Authenticate via Service Account key (`GCP_SA_KEY`) |
| Docker Auth | Login to Artifact Registry using OAuth2 access token |
| Build & Push | Build image tagged with `github.sha`, push to Artifact Registry |
| Deploy | Deploy image to Cloud Run service `flask-app` in `us-central1` |
| Output | Print the live Cloud Run service URL |

Each deployment is tagged with the commit SHA for full traceability.

## Environment Variables (Workflow)

| Variable | Value |
|----------|-------|
| `PROJECT_ID` | `spheric-keel-500501-s7` |
| `REGION` | `us-central1` |
| `REPOSITORY` | `flaskrepos` |
| `SERVICE` | `flask-app` |

## Secret Required

| Secret | Description |
|--------|-------------|
| `GCP_SA_KEY` | GCP Service Account JSON key with Cloud Run and Artifact Registry permissions |

## Key Concepts

**Commit SHA Tagging** — Every Docker image is tagged with the Git commit SHA, ensuring each deployment is uniquely traceable to its source code version.

**Serverless Deployment** — Cloud Run automatically manages scaling, eliminating the need to provision or manage infrastructure.

**OAuth2 Token Auth** — The pipeline uses a short-lived OAuth2 access token (derived from the Service Account) to authenticate with Artifact Registry — more secure than long-lived credentials.
