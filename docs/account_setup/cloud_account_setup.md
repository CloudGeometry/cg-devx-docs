# CG DevX AWS PrerequisitesCG DevX

Before you can start using CG DevX on AWS, you need to complete the following prerequisites:

### 1. Create an AWS Account

To create an AWS account with billing enabled, follow these steps:

- Go to the [AWS website](https://aws.amazon.com/) and click on the "Create an AWS Account" button.
- Follow the on-screen instructions to set up your AWS account.
- Enable billing for your account to ensure you can use the required AWS services.

### 2. Set Up a Public Hosted Zone with DNS Routing 

To establish a public hosted zone with DNS routing in AWS Route 53, perform the following steps ([docs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/AboutHZWorkingWith.html)):

- Log in to the AWS Management Console.
- Navigate to the AWS Route 53 service.
- Follow the AWS documentation to create a public hosted zone and configure DNS routing.

### 3. Configure IAM Credentials

To connect with Administrator Access IAM credentials to your AWS account, follow these steps ([docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html#access-keys-and-secret-access-keys)):

- Log in to the AWS Management Console.
- Go to the AWS Identity and Access Management (IAM) service.
- Create an IAM user with Administrator Access permissions.
- Make note of the Access Key ID and Secret Access Key for the IAM user.


### 4. Install AWS CLI

Before you begin, ensure that AWS CLI is installed on your system. You can check the installation by opening your terminal (command prompt) and typing:

```bash
aws --version
```

If AWS CLI is not installed, follow the appropriate instructions for your operating system:

For Mac:

If you have Homebrew installed, you can use it to install AWS CLI. Open your terminal and run the following command:

```bash
brew install awscli
```

For Linux (Ubuntu/Debian):

On Ubuntu or Debian, you can use the package manager to install AWS CLI. Open your terminal and run the following commands:

```bash
sudo apt-get update
sudo apt-get install awscli
```

For other Linux distributions, refer to the package manager documentation for installing AWS CLI.

### 5. Obtain AWS Access Key and Secret Access Key

To access AWS resources, you need an Access Key ID and a Secret Access Key. Follow these steps to obtain them:

1. Sign in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Open the "IAM" (Identity and Access Management) service.
3. In the left navigation pane, click on "Users."
4. Search and select your IAM user or create a new one.
5. In the "Security credentials" tab, click on "Create access key."
6. Note down the Access Key ID and Secret Access Key that appears. Make sure to save them securely.

### 6. Configure AWS CLI with CG DevX Credentials

Now, let's configure AWS CLI with the Access Key ID and Secret Access Key. Open your terminal (command prompt) and type the following command:

```bash
aws configure
```

You will be prompted to enter the following information:

1. AWS Access Key ID: Enter the Access Key ID you obtained in Step 2.
2. AWS Secret Access Key: Enter the Secret Access Key you obtained in Step 2.
3. Default region name: Enter the default AWS region you want to use (e.g., us-west-2).
4. Default output format: Enter the desired output format (e.g., json).

For example, on Mac:

```bash
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: us-west-2
Default output format [None]: json
```

For Linux:

```bash
AWS Access Key ID: YOUR_ACCESS_KEY_ID
AWS Secret Access Key: YOUR_SECRET_ACCESS_KEY
Default region name: us-west-2
Default output format: json
```

### 7. Verify the Configuration

To ensure that the AWS CLI is correctly configured with CG DevX credentials, you can run a simple command:

```bash
aws s3 ls
```

If the configuration is successful, you should see a list of your S3 buckets.

For more information and detailed documentation on AWS CLI, you can refer to the [official AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

Congratulations! You have now configured AWS CLI for CG DevX on both Mac and Linux, and you can start using it to manage your AWS resources seamlessly from the command line. Happy cloud computing!


### 8. Install AWS CLI

Before you begin, ensure that AWS CLI is installed on your system. You can check the installation by opening your terminal or command prompt and typing:

```
aws --version
```

If AWS CLI is not installed, you can download and install it from the [official AWS CLI website](https://aws.amazon.com/cli/).

### 9. Obtain AWS Access Key and Secret Access Key

To access AWS resources, you need an Access Key ID and a Secret Access Key. Follow these steps to obtain them:

1. Sign in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Open the "IAM" (Identity and Access Management) service.
3. In the left navigation pane, click on "Users."
4. Search and select your IAM user or create a new one.
5. In the "Security credentials" tab, click on "Create access key."
6. Note down the Access Key ID and Secret Access Key that appears. Make sure to save them securely.

### 10. Configure AWS CLI with CG DevX Credentials

Now, let's configure AWS CLI with the Access Key ID and Secret Access Key:

Open your terminal or command prompt and type the following command:

```
aws configure
```

You will be prompted to enter the following information:

1. AWS Access Key ID: Enter the Access Key ID you obtained in Step 5.
2. AWS Secret Access Key: Enter the Secret Access Key you obtained in Step 5.
3. Default region name: Enter the default AWS region you want to use (e.g., us-west-2).
4. Default output format: Enter the desired output format (e.g., JSON).

### 11. Verify the Configuration

To ensure that the AWS CLI is correctly configured with CG DevX credentials, you can run a simple command:
CG DevX
```
aws s3 ls
```

If the configuration is successful, you should see a list of your S3 buckets.

For more information and detailed documentation on AWS CLI, you can refer to the [official AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

If you prefer to use another installation method, refer to the AWS documentation for instructions on how to install the AWS CLI.

With these prerequisites completed, you are ready to proceed with the installation and configuration of CG DevX on AWS.