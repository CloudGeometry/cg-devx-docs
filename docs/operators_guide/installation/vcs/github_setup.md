# Using GitHub

When using GitHub as VCS, ensure that you have:

1. A user account used by automation.
   This account will be used to create and manage repositories,
   make commits, manage users, and perform other tasks, such as executing Terraform scripts.
   You can
   follow [this guide](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account).
2. A GitHub Organization.
   Organizations are used to group repositories, and CG DevX will create a new repo within a
   specific organization so that it's easy to find and manage later should you decide to stop using CGDevX.
   If you don't have one, please
   follow [this guide](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch)
   to create one.
   The user from step 1 should be a member of this organization.
3. A personal access token for the account from step 1.
   This token will enable CG DevX to take action on the user's
   behalf, creating and managing repos, and so on. To get a personal access token, also known as a "developer token",
   please
   follow the steps as described
   in [this guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token).

   The GitHub token will be used to authenticate with the GitHub API and perform various actions such as creating and
   deleting repositories, groups, and other users. To provide permission for these actions, make sure you select the
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