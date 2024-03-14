# Using GitLab

[//]: # (TODO: prepare configuration guide)

1. Create or use an existing [GitLab](https://about.gitlab.com/) account.
2. Create a [GitLab group](https://docs.gitlab.com/ee/user/group/) with `developer` permissions.
3. Create a GitLab personal access token following the steps described [here](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#create-a-personal-access-token).
   The token will be used to authenticate with the GitLab API and perform various actions such as creating and
   deleting repositories, groups, and other users.
   To provide permission for these actions, make sure you select the following set of scopes:

    - [x] **api** Grants complete read/write access to the API, including all groups and projects, the container
      registry, and the package registry.