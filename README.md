# al-agent-pipelines

> Reusable GitHub Actions pipeline templates for BC AL development.  
> Built on BcContainerHelper / bcartifacts.  
> Currently: GitHub-hosted runners — ready for self-hosted migration.

---

## Templates

| Template | Trigger | Purpose |
|---|---|---|
| `pr-validation.yml` | Pull Request to `main` | Compile + analyse AL code |
| `build-al.yml` | Push to `main` / manual | Full AL build + produce `.app` artifact |
| `test-al.yml` | Push to `main` / manual | Run AL test codeunits in BC container |
| `publish-appsource.yml` | Manual / release branch | Publish `.app` to AppSource or BC environment |

---

## How to use in an AL project

Copy the relevant template(s) into your AL project repo:

```
{al-project}/
└── .github/
    └── workflows/
        ├── pr-validation.yml
        ├── build-al.yml
        ├── test-al.yml
        └── publish-appsource.yml
```

Then configure the required secrets in your GitHub repo:

```
Settings → Secrets and variables → Actions → New repository secret
```

---

## Required secrets

| Secret | Purpose | Required by |
|---|---|---|
| `BC_TENANT_ID` | Azure AD tenant ID | All pipelines |
| `BC_CLIENT_ID` | App Registration client ID | `publish-appsource.yml` |
| `BC_CLIENT_SECRET` | App Registration client secret | `publish-appsource.yml` |
| `BC_ENVIRONMENT_NAME` | Target BC environment name | `publish-appsource.yml` |

---

## Runner migration guide

These pipelines use `runs-on: ubuntu-latest` (GitHub-hosted).  
To migrate to self-hosted:

1. Replace `runs-on: ubuntu-latest` with `runs-on: self-hosted`
2. Ensure Docker is installed on the self-hosted runner
3. Ensure BcContainerHelper is pre-installed or installed via pipeline
4. No other changes required — BcContainerHelper works identically on both

---

## BC artifact versioning strategy

Pipelines use `latest` BC artifacts by default to always build against the current BC version.  
To pin to a specific version, set the `BC_ARTIFACT_VERSION` variable in the workflow or repo variables.

---

## Tools used

| Tool | Version | Purpose |
|---|---|---|
| BcContainerHelper | latest | BC container management, AL build, AL test |
| bcartifacts | latest | BC artifact download |
| PowerShell | 7.x | Pipeline scripting |
