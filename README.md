# Google Cloud Run - GitOps

GitOps repository for managing Cloud Run service configurations across environments.

Each environment (development, staging, production) has its own folder with a `service.yaml` that defines the Cloud Run service spec. To deploy, go to Actions, run the "Promote Image" workflow, and fill in the image tag and target environment. The workflow will open a PR with the updated service.yaml automatically.

PRs that touch more than one environment at once will be rejected by the validation check. Changes per environment must go through separate PRs. Staging and production also require approval from `infra-team` before merge, enforced via CODEOWNERS.

On merge to `main`, the CD pipeline detects which environment folder changed and deploys only that service. Development deploys automatically. Staging and production go through a manual approval gate in GitHub Environments.

Before using this repository, make sure the following are in place: Workload Identity Federation between GitHub Actions and GCP, repository secrets `WIF_PROVIDER` and `WIF_SERVICE_ACCOUNT`, repository variable `GCP_PROJECT_ID`, and GitHub Environments set up for development, staging, and production.
