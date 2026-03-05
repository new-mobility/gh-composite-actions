# build-push-ecr

Composite action: build a Docker image and push it to AWS ECR using OIDC (no long-lived secrets).

**Reference:** `owner/gh-composite-actions/actions/build-push-ecr@main` (or `@v1`).

## Inputs

| Input | Required | Default | Description |
|-------|----------|--------|-------------|
| `ecr-repository` | yes | - | ECR repository name |
| `aws-role-arn` | yes | - | IAM role ARN for OIDC |
| `aws-region` | no | `us-east-1` | AWS region |
| `tag-with-sha` | no | `true` | Tag image with short commit hash (7 chars) |
| `tag-with-latest` | no | `true` | Tag image as `latest` |

## Example

```yaml
permissions:
  contents: read
  id-token: write

steps:
  - uses: actions/checkout@v4
  - uses: owner/gh-composite-actions/actions/build-push-ecr@main
    with:
      ecr-repository: my-service-name
      aws-role-arn: ${{ vars.AWS_ROLE_ARN }}
      aws-region: ${{ vars.AWS_REGION || 'us-east-1' }}
```

Replace `owner` with your GitHub org. Pin to a tag (e.g. `@v1`) for stable versions.

---

## Tag on push to main

A workflow (`.github/workflows/tag-on-push.yml`) creates a semantic version tag on every push to `main`, using [conventional commits](https://www.conventionalcommits.org/):

- `fix:` → patch (e.g. `1.0.0` → `1.0.1`)
- `feat:` → minor (e.g. `1.0.0` → `1.1.0`)
- `feat!:` or `BREAKING CHANGE` → major (e.g. `1.0.0` → `2.0.0`)

If there are no version-bumping commits since the last tag, no new tag is created.
