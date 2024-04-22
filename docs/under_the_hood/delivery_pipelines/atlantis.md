# Atlantis

Atlantis is used as PR automation tool for IaC part of GitOps. The goal is to:

- Centralize IaC execution
- Allow revocation of write access to infrastructure from the team
- Provide visibility on IaC changes and serve as audit log

As Git (users, teams, repositories) and cloud infrastructure are managed using IaC, Atlantis must have sufficient
permission to manage them.

### Git Access

To manage Git Atlantis is using a user associated with Git access token provided during setup process.
This token is stored as a secret in Vault during platform provisioning proces.
Atlantis reads secrets including ATLANTIS_GH_TOKEN from environment variables, which are loaded from Vault
by external secret operator from `atlantis-envs-secrets`.
Full list of variables used by Atlantis is defined in your platform GitOps
repository `platform/terraform/modules/secrets_vault/secrets.tf`

The concept is described in the Atlantis documentation and GitHub guidance:

> Creating an Atlantis User (Optional): It's recommended to create a new user named @atlantis or use a dedicated CI user
> for clarity, although this is not mandatory. You can also use an existing user or GitHub app credentials. The
> important
> aspect is that Atlantis will perform actions and write comments under this user's name, so it should be recognizable
> and
> not confuse the workflow.

In case you want to use a different user, you need to create new user (or use existing user) and access token. This
token will replace exiting user token, and should be manually updates in Vault. For mor details on secret management
please [see](../../operators_guide/platform_management/services/secrets.md).

Suggested token scope is "`Administration: Read-only`" and includes following permissions:

- Checks: Read and write.
- Commit statuses: Read and write.
- Contents: Read and write.
- Issues: Read and write.
- Metadata: Read-only (default).
- Pull requests: Read and write.
- Webhooks: Read and write.
- Members: Read-only.

To get a personal access token, also known as a "developer token", please
follow the steps as described
in [GitHub guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token).

For the most accurate and up-to-date information, you should consult the Atlantis documentation and the GitHub
documentation for creating personal access tokens as these resources will have the latest guidelines and best practices
for setting up Atlantis with GitHub.

For more details on permission please
check [setup guide](../../operators_guide/installation/quickstart.md#configure-git-provider)

## Cloud Access

Atlantis role for cloud provider access is managed via service account mechanism applied with annotation(s). For more
details on role mappings please see [IAM](../../under_the_hood/iam/iam.md)

Default role permissions should be adjusted based on your specific
needs. [AWS IAM Access Analyzer](https://docs.aws.amazon.com/en_us/IAM/latest/UserGuide/what-is-access-analyzer.html) or
similar tools could be used to evaluate required permissions scope.
