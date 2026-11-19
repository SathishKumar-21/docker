# flask-app

Small Flask API (`/` and `/health`) built as a **multi-stage** image:
`python:3.12` builder installs deps → `python:3.12-slim` runtime copies only what it needs.

## Run locally (with Postgres)

```bash
cp .env.example .env          # then edit POSTGRES_PASSWORD
docker compose up -d --build
curl localhost:5000/health
docker compose down           # add -v to also delete the pg-data volume
```

Compose reads `.env` from this folder for `${VAR}` substitution. A missing
`POSTGRES_PASSWORD` stops startup with an error instead of running with a blank password.

## Push to Azure Container Registry

```bash
az acr login -n flaskdevacrsathish24
docker build -t flaskdevacrsathish24.azurecr.io/flask-app:v2 .
docker push flaskdevacrsathish24.azurecr.io/flask-app:v2
```

| Tag | Meaning |
|---|---|
| `v2` | Working image, used by the AKS deployment |
| `v3` | Deliberately broken build (`/health` returns 500), used to practise CrashLoopBackOff and `kubectl rollout undo` |

Registry is created by [terraform/azure-flask](https://github.com/SathishKumar-21/Terraform/tree/main/azure-flask);
the image is deployed by [kubernetes/flask/aks](https://github.com/SathishKumar-21/kubernetes/tree/main/flask/aks).
