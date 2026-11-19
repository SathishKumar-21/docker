# Docker

Container images. One folder per app.

| Folder | What |
|---|---|
| [flask-app/](flask-app/) | Flask API, multi-stage Python image + docker compose with Postgres. Pushed to ACR. |

Related repos:
- [Terraform](https://github.com/SathishKumar-21/Terraform): Azure infra (network, ACR, AKS)
- [kubernetes](https://github.com/SathishKumar-21/kubernetes): manifests that run these images on AKS
