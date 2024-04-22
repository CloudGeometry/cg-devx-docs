# Atlantis

## Prerequisites

### Using with GitHub

To configure Atlantis with GitHub, you'll need to set up an access token with specific permissions to ensure Atlantis can perform its functions properly, such as commenting on pull requests, managing pull requests, setting commit statuses, and more. Here's a summary based on the Atlantis documentation and GitHub guidance:

Creating an Atlantis User (Optional): It's recommended to create a new user named @atlantis or use a dedicated CI user for clarity, although this is not mandatory. You can also use an existing user or GitHub app credentials. The important aspect is that Atlantis will perform actions and write comments under this user's name, so it should be recognizable and not confuse the workflow​​.

Generating an Access Token for a GitHub User:

The token needs the repo scope to operate correctly within your repositories.

If you're using Atlantis in an organizational context, ensure that the Atlantis user has "Write permissions" for repositories within the organization, or is a "Collaborator" on repositories in a personal account. This is necessary for setting commit statuses​​.

Permissions for GitHub App: If you choose to use Atlantis as a GitHub App, it requires specific permissions to function:

`Administration: Read-only`

- Checks: Read and write.

- Commit statuses: Read and write.

- Contents: Read and write.

- Issues: Read and write.

- Metadata: Read-only (default).

- Pull requests: Read and write.

- Webhooks: Read and write.

- Members: Read-only​​.

These permissions are critical for the Atlantis GitHub App to interact with your repositories, handle webhooks, manage issues, pull requests, and more. Note that if you've updated Atlantis to a newer version, some permissions (like Administration and Members) might not be automatically added and would need to be manually set if you're upgrading from an older version.

For the most accurate and up-to-date information, you should consult the Atlantis documentation and the GitHub documentation for creating personal access tokens as these resources will have the latest guidelines and best practices for setting up Atlantis with GitHub​​.

Currently, a personal access token for the account enable CGDevX to take action on the user'sbehalf,
creating and managing repos, and so on. To get a personal access token, also known as a "developer token", please
follow the steps as described
in [this guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token).

The GitHub token will be used to authenticate with the GitHub API and perform various actions such as creating and
deleting repositories, groups, and other users. To provide permission for these actions, make sure you seelct the
following set of scopes:

    - [x] **repo** Full control of private repositories
        - [x] **repo:status** Access commit status
        - [x] **repo_deployment** Access deployment status
        - [x] **public_repo** Access public repositories
        - [x] **repo:invite** Access repository invitations
        - [x] **security_events** Read and write security events
    - [x] **workflow** Update GitHub Action workflows
    - [x] **write:packages** Upload packages to GitHub Package Registry
        - [x] **read:packages** Download packages from GitHub Package Registry
    - [x] **admin:org** Full control of orgs and teams, read and write org projects
        - [x] **write:org** Read and write org and team membership, read and write org projects
        - [x] **read:org** Read org and team membership, read org projects
        - [x] **manage_runners:org** Manage org runners and runner groups
    - [x] **admin:public_key** Full control of user public keys
        - [x] **write:public_key** Write user public keys
        - [x] **read:public_key** Read user public keys
    - [x] **admin:repo_hook** Full control of repository hooks
        - [x] **write:repo_hook** Write repository hooks
        - [x] **read:repo_hook** Read repository hooks
    - [x] **admin:org_hook** Full control of organization hooks
    - [x] **user** Update ALL user data
        - [x] **read:user** Read ALL user profile data
        - [x] **user:email** Access user email addresses (read-only)
        - [x] **user:follow** Follow and unfollow users
    - [x] **delete_repo** Delete repositories
    - [x] **admin:ssh_signing_key** Full control of public user SSH signing keys
        - [x] **write:ssh_signing_key** Write public user SSH signing keys
        - [x] **read:ssh_signing_key** Read public user SSH signing keys

## Running Atlantis in the EKS cluster

Atlantis reads the secret from Vault - there are environment variables, including [ATLANTIS_GH_TOKEN](https://github.com/CloudGeometry/cg-devx-core/blob/3071a73d2af4db6272234c3e1b6727e1a18bec7d/platform/terraform/modules/secrets_vault/secrets.tf#L57)

Secrets - Antlantis - Env Secrets goes to envs inside of Atlantis pod with a service account that has annotations:

`eks.amazonaws.com/role-arn=arn:aws:iam::728538925474:role/cg-devx-aws-test-iac_pr_automation-role`

IRSA Trust Relationships inside the IAM role [${local.name}-iac_pr_automation-role](https://github.com/CloudGeometry/cg-devx-core/blob/3071a73d2af4db6272234c3e1b6727e1a18bec7d/platform/terraform/modules/cloud_aws/iam.tf#L81) with

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::728538925474:oidc-provider/oidc.eks.eu-west-1.amazonaws.com/id/784EAFFE74CD0BB4E3A00019B1D30B94"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "oidc.eks.eu-west-1.amazonaws.com/id/784EAFFE74CD0BB4E3A00019B1D30B94:aud": "sts.amazonaws.com",
                    "oidc.eks.eu-west-1.amazonaws.com/id/784EAFFE74CD0BB4E3A00019B1D30B94:sub": "system:serviceaccount:atlantis:atlantis"
                }
            }
        }
    ]
}
```

Here

`"oidc.eks.eu-west-1.amazonaws.com/id/784EAFFE74CD0BB4E3A00019B1D30B94:sub": "system:serviceaccount:atlantis:atlantis"`

means that it can be interconnected with the EKS role

Atlantis is a Stateful Set and has `envFrom` → `Atlantis-envs-secrets` taken from the external secret `atlantis-envs-secrets`

[Here](https://github.com/CloudGeometry/cg-devx-core/blob/3071a73d2af4db6272234c3e1b6727e1a18bec7d/platform/terraform/modules/secrets_vault/secrets.tf#L57), the secret `ATLANTIS_GH_TOKEN` is created by the Terraform and takes the parameter inside of `var.vcs_token` and `TF_VAR_vcs_token`, which has taken from the `var.vcs_token`

While running, the terraform code uses a GitHub provider, which is not configured. It underhood takes the GitHub token
