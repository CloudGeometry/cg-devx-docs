# Deploying on AWS

When deploying to AWS, ensure that you have:

1. An AWS account with billing enabled. (Remember, deploying clusters will incur charges. Make sure to destroy
   resources when you're finished with them!)
2. A public hosted zone with DNS routing.
   To set this up,
   you can follow [this guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/AboutHZWorkingWith.html).
3. A user account with `AdministratorAccess`. We recommend that rather than using your root account, you set up a
   new IAM user, then grant it AdministratorAccess. You can
   use [this guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
   to set up an IAM account,
   and [this guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) to grant it
   `AdministratorAccess`.
4. The security credentials for this account, which enables CGDevX to use it.
   Use [this guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html#access-keys-and-secret-access-keys)
   to get your access keys.
5. The AWS CLI installed and configured to use this user.
   You can use [this guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) to install
   the CLI.
