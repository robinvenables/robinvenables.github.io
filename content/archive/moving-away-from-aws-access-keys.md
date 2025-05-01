---
title: "Moving Away From AWS Access Keys"
date: 2023-05-16T10:00:00
categories: ["devops"]
tags: ["aws","github","security"]
series: ["archive"]
---

I started using [GitHub Actions][1] to handle the Hugo build and deployment of this website. I was using a dedicated AWS IAM user with the required policies and added their Access Key ID and Secret Access Key as [repository secrets][2]. The relevant step in my GitHub action looked like this:

```yaml
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
```

Every 90 days I would have to generate new keys in IAM and then update the repository secrets. What if I didn't have to do this? The answer is to configure an Identity Provider for GitHub in AWS IAM and then use OpenID Connect to authenticate in the GitHub workflow. You can add GitHub as an Identity Provider with a single CLI command:

```bash
$ aws iam create-open-id-connect-provider --url "https://token.actions.GitHubusercontent.com" \
  --thumbprint-list "6938fd4d98bab03faadb97b34396831e3780aea1" \
  --client-id-list "sts.amazonaws.com"
```

Next create a role for GitHub to assume, with the required trust relationships. For example:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::123456789ABC:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:sub": "repo:myorg/example.com:ref:refs/heads/main",
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                }
            }
        }
    ]
}
```

Then add the required permissions policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
          "Sid": "myGitHubActionsPolicy",
          "Effect": "Allow",
          "Action": [
              "s3:GetBucketPolicy",
              "s3:PutBucketPolicy",
              "s3:ListBucket",
              "s3:PutObject",
              "s3:DeleteObject",
              "cloudfront:CreateInvalidation"
          ],
          "Resource": [
              "arn:aws:s3:::example.com",
              "arn:aws:s3:::example.com/*",
              "arn:aws:cloudfront::123456789ABC:distribution/CLOUDFRONTID01"
          ]
      }
  ]
}
```

Finally, add a permissions stanza to the workflow to allow the workflow to write the identity token and update the 'Configure AWS Credentials' step. The full workflow should look something like this:

```yaml
name: Build and Deploy example.com

on:
  push:
    branches: [ "main" ]

  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v3

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: arn:aws:iam::123456789ABC:role/myGitHubActionsRole
          role-session-name:
          aws-region: eu-west-2

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.111.3'

      - name: Build website
        run: hugo --minify --verbose

      - name: Deploy to S3
        run: hugo deploy --maxDeletes -1 --invalidateCDN --verbose
```

Once this is tested and working the IAM user and the associated repository secrets can be deleted.

[1]: https://github.com/features/actions
[2]: https://docs.github.com/en/actions/security-guides/encrypted-secrets
