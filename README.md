# build-push-ecr

Composite action: build a Docker image and push it to AWS ECR using OIDC (no long-lived secrets).

**Reference:** `owner/gh-composite-actions/actions/build-push-ecr@main` (or `@v1`).

## Inputs

| Input | Required | Default | Description |
|-------|----------|--------|-------------|
| `ecr-repository` | yes | - | ECR repository name |
| `aws-role-arn` | yes | - | IAM role ARN for OIDC |
| `aws-region` | no | `us-east-1` | AWS region |
| `tag-with-sha` | no | `true` | Tag image with git SHA |
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
