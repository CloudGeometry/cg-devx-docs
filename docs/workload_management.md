# Creating a New Workload using CGDevX CLI

In this guide, we will explore how to create a new workload using the CGDevX Command Line Interface (CLI). The CLI provides a convenient and efficient way to interact with CGDevX and automate various tasks. By following the steps below, you'll be able to create a new workload with ease.

## Prerequisites

Before we begin, make sure you have the following:

- A CGDevX account with CLI access
- CGDevX CLI installed and properly configured on your local machine
- Basic familiarity with the command line interface

## Step 1: Launching the CGDevX CLI

To start the process of creating a new workload, open your preferred terminal application and launch the CGDevX CLI. Ensure that you are logged in with your CGDevX account credentials. If you haven't installed the CLI or set it up yet, please refer to the CGDevX documentation for installation instructions.

## Step 2: Running the workload-create Command

Once you have the CLI up and running, it's time to use the `workload-create` command to create your new workload. The `workload-create` command allows you to define various parameters for your workload, such as the workload name and optional repository details. Here's an example command:

```shell
cgdevx workload-create --workload-name <workload-name> --workloadrepository-name <workloadrepository-name> --workload-gitopsrepository-name <workload-gitopsrepository-name>
```

Make sure to replace `<workload-name>`, `<workloadrepository-name>`, and `<workload-gitopsrepository-name>` with the actual values you want to use. The `--workload-name` parameter specifies the name of your workload, while the `--workloadrepository-name` and `--workload-gitopsrepository-name` parameters are optional and allow you to associate a workload with specific repositories.


### Step 2.1: Providing Additional Input Parameters

In addition to the command options mentioned above, there are also hidden input parameters that you can use to customize your workload further. These parameters are related to workload templates and GitOps repositories. Although they are hidden, they play an important role in defining the behavior and configuration of your workload. Here are the hidden parameters:

- `--workloadtemplate-url`: Specifies the URL of the workload template.
- `--workloadtemplate-branch`: Specifies the branch of the workload template.
- `--workload-gitopstemplate-url`: Specifies the URL of the GitOps template.
- `--workload-gitopstemplate-branch`: Specifies the branch of the GitOps template.

These hidden parameters allow you to leverage predefined templates and configurations to streamline your workload creation process.

## Step 3: Verifying the Workload Creation

Once you've executed the `workload-create` command with the required and optional parameters, the CLI will initiate the workload creation process. It will provide feedback and status updates as it progresses. After completion, the CLI will display relevant information about the newly created workload, including its ID, name, and associated repositories (if specified).

## Step 4: Waiting for Provisioning to Complete

After executing the `workload-create` command, the CGDevX platform will begin provisioning your new workload. The provisioning process involves setting up the necessary infrastructure, resources, and configurations for your workload to run smoothly. The exact duration of the provisioning process may vary depending on various factors.

During this step, it is essential to exercise patience and allow the platform to complete the provisioning. The time required for provisioning depends on the complexity of your workload and the current load on the CGDevX platform. The platform may perform various tasks, such as resource allocation, network configuration, and dependency installation, to ensure your workload is ready for use.

It's important to note that you may encounter a waiting period where manual approval from the platform operator is required. This additional step ensures that the workload creation aligns with the platform's policies and guidelines. The platform operator will review your workload request and take any necessary actions to ensure its compliance and security.

To keep track of the provisioning progress, you can periodically check the status of your workload using the CGDevX CLI or the platform's web interface. Look for indicators or notifications that signal the completion of the provisioning process. Once the provisioning is complete, you can proceed with using and managing your workload as intended.

## Step 6: Bootstrapping Your Workload

Once the provisioning process for your workload is complete, you can proceed with bootstrapping it using the `workload-bootstrap` command in the CGDevX CLI. This step involves setting up the initial configurations and dependencies required for your workload to function properly.


```shell
cgdevx workload-bootstrap --workload-name <workload-name> --workloadrepository-name <workloadrepository-name> --workload-gitopsrepository-name <workload-gitopsrepository-name>
```

Make sure to replace `<workload-name>`, `<workloadrepository-name>`, and `<workload-gitopsrepository-name>` with the actual values you want to use. The `--workload-name` parameter specifies the name of your workload, while the `--workloadrepository-name` and `--workload-gitopsrepository-name` parameters are optional and allow you to associate a workload with specific repositories.
   
### Step 6.1: Providing Additional Input Parameters

In addition to the command options mentioned above, there are also hidden input parameters that you can use to customize your workload further. These parameters are related to workload templates and GitOps repositories. Although they are hidden, they play an important role in defining the behavior and configuration of your workload. Here are the hidden parameters:

- `--workloadtemplate-url`: Specifies the URL of the workload template.
- `--workloadtemplate-branch`: Specifies the branch of the workload template.
- `--workload-gitopstemplate-url`: Specifies the URL of the GitOps template.
- `--workload-gitopstemplate-branch`: Specifies the branch of the GitOps template.

These hidden parameters allow you to leverage predefined templates and configurations to streamline your workload creation process.

## Step 7: Wait for the bootstrapping process to complete. 

Once the bootstrapping process is finished, your workload will be ready for further customization and deployment. You can now proceed to the next steps to manage and operate your workload effectively.
