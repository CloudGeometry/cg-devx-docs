# Using GitHub

When using GitHub as your Version Control System (VCS), ensure that you have:

1. **A User Account for Automation**:
   This account will be used to create and manage repositories, make commits, manage users, and perform other tasks, such as executing Terraform scripts. You can follow [this guide](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account) to create a new GitHub account.
   
2. **A GitHub Organization**:
   Organizations are used to group repositories, and CG DevX will create new repositories within a specific organization so that it's easy to find and manage later should you decide to stop using CG DevX. If you don't have one, please follow [this guide](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch) to create one. The user from step 1 should be a member of this organization.
   
3. **A Personal Access Token for the Account from Step 1**:
   This token will enable CG DevX to take action on the user's behalf, such as creating and managing repositories. To get a personal access token, also known as a "developer token", please follow the steps described in [this guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token).

   The GitHub token will be used to authenticate with the GitHub API and perform various actions such as creating and deleting repositories, groups, and other users. To provide permission for these actions, make sure you select the following set of scopes:

    - **repo**: Full control of private repositories
      - **repo:status**: Access commit status
      - **repo_deployment**: Access deployment status
      - **public_repo**: Access public repositories
      - **repo:invite**: Access repository invitations
      - **security_events**: Read and write security events
    - **workflow**: Update GitHub Action workflows
    - **write:packages**: Upload packages to GitHub Package Registry
      - **read:packages**: Download packages from GitHub Package Registry
    - **admin:org**: Full control of orgs and teams, read and write org projects
      - **write:org**: Read and write org and team membership, read and write org projects
      - **read:org**: Read org and team membership, read org projects
      - **manage_runners:org**: Manage org runners and runner groups
    - **admin:public_key**: Full control of user public keys
      - **write:public_key**: Write user public keys
      - **read:public_key**: Read user public keys
    - **admin:repo_hook**: Full control of repository hooks
      - **write:repo_hook**: Write repository hooks
      - **read:repo_hook**: Read repository hooks
    - **admin:org_hook**: Full control of organization hooks
    - **user**: Update ALL user data
      - **read:user**: Read ALL user profile data
      - **user:email**: Access user email addresses (read-only)
      - **user:follow**: Follow and unfollow users
    - **delete_repo**: Delete repositories
    - **admin:ssh_signing_key**: Full control of public user SSH signing keys
      - **write:ssh_signing_key**: Write public user SSH signing keys
      - **read:ssh_signing_key**: Read public user SSH signing keys
