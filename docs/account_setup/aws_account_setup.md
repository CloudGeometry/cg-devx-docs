# CGDevX AWS Prerequisites

Before you can start using CGDevX on AWS, you need to complete the following prerequisites:

## 1. Create an AWS Account

To create an AWS account with billing enabled, follow these steps:

- Go to the [AWS website](https://aws.amazon.com/) and click on the "Create an AWS Account" button.
- Follow the on-screen instructions to set up your AWS account.
- Enable billing for your account to ensure you can use the required AWS services.

## 2. Set Up a Public Hosted Zone with DNS Routing 

To establish a public hosted zone with DNS routing in AWS Route 53, perform the following steps ([docs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/AboutHZWorkingWith.html)):

- Log in to the AWS Management Console.
- Navigate to the AWS Route 53 service.
- Follow the AWS documentation to create a public hosted zone and configure DNS routing.

## 3. Configure IAM Credentials

To connect with Administrator Access IAM credentials to your AWS account, follow these steps ([docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html#access-keys-and-secret-access-keys)):

- Log in to the AWS Management Console.
- Go to the AWS Identity and Access Management (IAM) service.
- Create an IAM user with Administrator Access permissions.
- Make note of the Access Key ID and Secret Access Key for the IAM user.

## 4. Install the AWS CLI and AWS IAM Authenticator

CGDevX requires the AWS CLI and AWS IAM Authenticator utilities. Here's how to install them using Homebrew ([docs](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)):

- Install Homebrew by following the instructions on the [Homebrew website](https://brew.sh/).
- Open a terminal.
- Run the following command to install the AWS CLI and AWS IAM Authenticator:

```shell
brew install awscli
```

If you prefer to use another installation method, refer to the AWS documentation for instructions on how to install the AWS CLI and AWS IAM Authenticator utilities.

With these prerequisites completed, you are ready to proceed with the installation and configuration of CGDevX on AWS.